//! CKB SPHINCS+ Lock Script Wrapper
//!
//! Production-ready interface to the deployed SPHINCS+ lock script on CKB.
//! Uses the lock at:
//!   Testnet: code_hash 0x147ecbb5c5127d982ee1362d2c2bb4267803da2eb006d150e88af6caaa0a7eaf
//!   Mainnet: code_hash 0x302d35982f865ebcbedb9a9360e40530ed32adb8e10b42fbbe70d8312ff7cedf

use crate::error::SovIdError;
use sha2::{Digest, Sha256};
use blake2b_simd::Params as Blake2bParams;

/// CKB network configuration
#[derive(Clone, Debug)]
pub enum CkbNetwork {
    Testnet,
    Mainnet,
}

impl CkbNetwork {
    pub fn rpc_url(&self) -> &str {
        match self {
            CkbNetwork::Testnet => "https://testnet.ckb.dev/",
            CkbNetwork::Mainnet => "https://mainnet.ckb.dev/",
        }
    }

    pub fn code_hash(&self) -> &str {
        match self {
            CkbNetwork::Testnet => "0x147ecbb5c5127d982ee1362d2c2bb4267803da2eb006d150e88af6caaa0a7eaf",
            CkbNetwork::Mainnet => "0x302d35982f865ebcbedb9a9360e40530ed32adb8e10b42fbbe70d8312ff7cedf",
        }
    }

    pub fn hash_type(&self) -> &str {
        match self {
            CkbNetwork::Testnet => "data1",
            CkbNetwork::Mainnet => "type",
        }
    }

    pub fn tx_hash(&self) -> &str {
        match self {
            CkbNetwork::Testnet => "0x631d9a6049fb1fc3790e89d9daf35abe535b5e754cd8c3404319319710f0b106",
            CkbNetwork::Mainnet => "0x4598d00df2f3dc8bc40eee38689a539c94f6cc3720b7a2a6746736daa60f500a",
        }
    }

    pub fn out_point_index(&self) -> &str {
        "0x0"
    }
}

/// Parameter set for SLH-DSA
#[derive(Clone, Copy, Debug)]
pub enum SphincsParamSet {
    Sha2_256s,
    Sha2_192s,
    Sha2_128s,
    Shake_256s,
}

impl SphincsParamSet {
    /// 5-byte prefix for script args (blake2b hashing)
    pub fn args_prefix(&self) -> [u8; 5] {
        match self {
            SphincsParamSet::Sha2_256s => [0x80, 0x01, 0x01, 0x01, 0x6a],
            SphincsParamSet::Sha2_192s => [0x80, 0x01, 0x01, 0x01, 0x66],
            SphincsParamSet::Sha2_128s => [0x80, 0x01, 0x01, 0x01, 0x62],
            SphincsParamSet::Shake_256s => [0x80, 0x01, 0x01, 0x01, 0x76],
        }
    }

    /// 5-byte prefix for witness
    pub fn witness_prefix(&self) -> [u8; 5] {
        match self {
            SphincsParamSet::Sha2_256s => [0x80, 0x01, 0x01, 0x01, 0x6b],
            SphincsParamSet::Sha2_192s => [0x80, 0x01, 0x01, 0x01, 0x67],
            SphincsParamSet::Sha2_128s => [0x80, 0x01, 0x01, 0x01, 0x63],
            SphincsParamSet::Shake_256s => [0x80, 0x01, 0x01, 0x01, 0x77],
        }
    }
}

impl Default for SphincsParamSet {
    fn default() -> Self {
        SphincsParamSet::Sha2_256s
    }
}

/// Calculate the script args for a SPHINCS+ lock
///
/// script_args = blake2b(prefix || public_key, personal="ckb-sphincs+-sct")
pub fn calculate_script_args(
    params: SphincsParamSet,
    public_key: &[u8],
) -> Vec<u8> {
    let prefix = params.args_prefix();
    let mut preimage = Vec::with_capacity(prefix.len() + public_key.len());
    preimage.extend_from_slice(&prefix);
    preimage.extend_from_slice(public_key);

    let mut hasher = Blake2bParams::new()
        .hash_length(32)
        .personal(b"ckb-sphincs+-sct")
        .to_state();
    hasher.update(&preimage);
    hasher.finalize().as_bytes().to_vec()
}

/// Build the witness lock field for a SPHINCS+ transaction
///
/// witness_lock = prefix || public_key || signature
pub fn build_witness_lock(
    params: SphincsParamSet,
    public_key: &[u8],
    signature: &[u8],
) -> Vec<u8> {
    let prefix = params.witness_prefix();
    let mut witness = Vec::with_capacity(prefix.len() + public_key.len() + signature.len());
    witness.extend_from_slice(&prefix);
    witness.extend_from_slice(public_key);
    witness.extend_from_slice(signature);
    witness
}

/// CKB lock script deployment info
#[derive(Clone, Debug)]
pub struct SphincsLock {
    pub network: CkbNetwork,
    pub param_set: SphincsParamSet,
}

impl SphincsLock {
    pub fn new(network: CkbNetwork, param_set: SphincsParamSet) -> Self {
        Self { network, param_set }
    }

    pub fn default_testnet() -> Self {
        Self::new(CkbNetwork::Testnet, SphincsParamSet::default())
    }

    pub fn default_mainnet() -> Self {
        Self::new(CkbNetwork::Mainnet, SphincsParamSet::default())
    }

    /// Build the cell_dep for this lock (to include in transactions)
    pub fn cell_dep(&self) -> serde_json::Value {
        serde_json::json!({
            "out_point": {
                "tx_hash": self.network.tx_hash(),
                "index": self.network.out_point_index()
            },
            "dep_type": "code"
        })
    }

    /// Build the lock script for a given public key
    pub fn lock_script(&self, public_key: &[u8]) -> serde_json::Value {
        let args = calculate_script_args(self.param_set, public_key);
        serde_json::json!({
            "code_hash": self.network.code_hash(),
            "hash_type": self.network.hash_type(),
            "args": format!("0x{}", hex::encode(&args)),
        })
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_script_args_deterministic() {
        let pk = [0x42u8; 32];
        let args1 = calculate_script_args(SphincsParamSet::Sha2_256s, &pk);
        let args2 = calculate_script_args(SphincsParamSet::Sha2_256s, &pk);
        assert_eq!(args1, args2);
        assert_eq!(args1.len(), 32); // blake2b 32-byte output
    }

    #[test]
    fn test_different_params_different_args() {
        let pk = [0x42u8; 32];
        let a = calculate_script_args(SphincsParamSet::Sha2_256s, &pk);
        let b = calculate_script_args(SphincsParamSet::Sha2_128s, &pk);
        assert_ne!(a, b); // Different prefix → different hash
    }

    #[test]
    fn test_lock_script_json() {
        let lock = SphincsLock::default_testnet();
        let script = lock.lock_script(&[0x42u8; 32]);
        assert_eq!(script["hash_type"], "data1");
        assert!(script["args"].as_str().unwrap().starts_with("0x"));
    }

    #[test]
    fn test_witness_format() {
        let pk = [0x42u8; 32];
        let sig = [0x99u8; 29792]; // SLH-DSA-SHA2-256s sig length
        let witness = build_witness_lock(SphincsParamSet::Sha2_256s, &pk, &sig);
        assert_eq!(witness.len(), 5 + 32 + 29792);
        assert_eq!(&witness[0..5], &[0x80, 0x01, 0x01, 0x01, 0x6b]); // witness prefix
    }
}