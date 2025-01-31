<!-- Code generated by gomarkdoc. DO NOT EDIT -->

# ted25519

```go
import "github.com/coinbase/kryptology/pkg/ted25519/ted25519"
```

## Index

- [Constants](<#constants>)
- [func ExpandSeed(seed []byte) []byte](<#func-expandseed>)
- [func GenerateKey(rand io.Reader) (PublicKey, PrivateKey, error)](<#func-generatekey>)
- [func GenerateSharedKey(config *ShareConfiguration) (PublicKey, []*KeyShare, Commitments, error)](<#func-generatesharedkey>)
- [func GenerateSharedNonce(config *ShareConfiguration, s *KeyShare, p PublicKey, m Message) (PublicKey, []*NonceShare, Commitments, error)](<#func-generatesharednonce>)
- [func PublicKeyFromBytes(bytes []byte) ([]byte, error)](<#func-publickeyfrombytes>)
- [func Reconstruct(keyShares []*KeyShare, config *ShareConfiguration) ([]byte, error)](<#func-reconstruct>)
- [func Sign(privateKey PrivateKey, message []byte) ([]byte, error)](<#func-sign>)
- [func ThresholdSign(expandedSecretKeyShare []byte, publicKey PublicKey, message []byte, rShare []byte, R PublicKey) []byte](<#func-thresholdsign>)
- [func Verify(publicKey PublicKey, message, sig []byte) (bool, error)](<#func-verify>)
- [type Commitments](<#type-commitments>)
  - [func CommitmentsFromBytes(bytes [][]byte) (Commitments, error)](<#func-commitmentsfrombytes>)
  - [func (commitments Commitments) CommitmentsToBytes() [][]byte](<#func-commitments-commitmentstobytes>)
- [type KeyShare](<#type-keyshare>)
  - [func KeyShareFromBytes(bytes []byte) *KeyShare](<#func-keysharefrombytes>)
  - [func NewKeyShare(identifier byte, secret []byte) *KeyShare](<#func-newkeyshare>)
  - [func (share *KeyShare) VerifyVSS(commitments Commitments, config *ShareConfiguration) (bool, error)](<#func-keyshare-verifyvss>)
- [type Message](<#type-message>)
  - [func (m Message) String() string](<#func-message-string>)
- [type NonceShare](<#type-nonceshare>)
  - [func NewNonceShare(identifier byte, secret []byte) *NonceShare](<#func-newnonceshare>)
  - [func NonceShareFromBytes(bytes []byte) *NonceShare](<#func-noncesharefrombytes>)
  - [func (n NonceShare) Add(other *NonceShare) *NonceShare](<#func-nonceshare-add>)
- [type PartialSignature](<#type-partialsignature>)
  - [func NewPartialSignature(identifier byte, sig []byte) *PartialSignature](<#func-newpartialsignature>)
  - [func TSign(message Message, key *KeyShare, pub PublicKey, nonce *NonceShare, noncePub PublicKey) *PartialSignature](<#func-tsign>)
  - [func (sig *PartialSignature) Bytes() []byte](<#func-partialsignature-bytes>)
  - [func (sig *PartialSignature) R() []byte](<#func-partialsignature-r>)
  - [func (sig *PartialSignature) S() []byte](<#func-partialsignature-s>)
- [type PrivateKey](<#type-privatekey>)
  - [func NewKeyFromSeed(seed []byte) (PrivateKey, error)](<#func-newkeyfromseed>)
  - [func (priv PrivateKey) Public() crypto.PublicKey](<#func-privatekey-public>)
  - [func (priv PrivateKey) Seed() []byte](<#func-privatekey-seed>)
  - [func (priv PrivateKey) Sign(rand io.Reader, message []byte, opts crypto.SignerOpts) (signature []byte, err error)](<#func-privatekey-sign>)
- [type PublicKey](<#type-publickey>)
  - [func GeAdd(a PublicKey, b PublicKey) PublicKey](<#func-geadd>)
  - [func (p PublicKey) Bytes() []byte](<#func-publickey-bytes>)
- [type ShareConfiguration](<#type-shareconfiguration>)
- [type Signature](<#type-signature>)
  - [func Aggregate(sigs []*PartialSignature, config *ShareConfiguration) (Signature, error)](<#func-aggregate>)


## Constants

```go
const (
    // PublicKeySize is the size, in bytes, of public keys as used in this package.
    PublicKeySize = 32
    // PrivateKeySize is the size, in bytes, of private keys as used in this package.
    PrivateKeySize = 64
    // SignatureSize is the size, in bytes, of signatures generated and verified by this package.
    SignatureSize = 64
    // SeedSize is the size, in bytes, of private key seeds. These are the private key representations used by RFC 8032.
    SeedSize = 32
)
```

## func [ExpandSeed](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ext.go#L32>)

```go
func ExpandSeed(seed []byte) []byte
```

ExpandSeed applies the standard Ed25519 transform to the seed to turn it into the real private key that is used for signing\. It returns the expanded seed\.

## func [GenerateKey](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L87>)

```go
func GenerateKey(rand io.Reader) (PublicKey, PrivateKey, error)
```

GenerateKey generates a public/private key pair using entropy from rand\. If rand is nil\, crypto/rand\.Reader will be used\.

## func [GenerateSharedKey](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L119>)

```go
func GenerateSharedKey(config *ShareConfiguration) (PublicKey, []*KeyShare, Commitments, error)
```

GenerateSharedKey generates a random key\, splits it\, and returns the public key\, shares\, and VSS commitments\.

## func [GenerateSharedNonce](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/noncegen.go#L68-L73>)

```go
func GenerateSharedNonce(config *ShareConfiguration, s *KeyShare, p PublicKey, m Message) (PublicKey, []*NonceShare, Commitments, error)
```

GenerateSharedNonce generates a random nonce\, splits it\, and returns the nonce pubkey\, nonce shares\, and VSS commitments\.

## func [PublicKeyFromBytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L17>)

```go
func PublicKeyFromBytes(bytes []byte) ([]byte, error)
```

PublicKeyFromBytes converts byte array into PublicKey byte array

## func [Reconstruct](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L179>)

```go
func Reconstruct(keyShares []*KeyShare, config *ShareConfiguration) ([]byte, error)
```

Reconstruct recovers the secret from a set of secret shares\.

## func [Sign](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L149>)

```go
func Sign(privateKey PrivateKey, message []byte) ([]byte, error)
```

Sign signs the message with privateKey and returns a signature\. It will panic if len\(privateKey\) is not PrivateKeySize\.

## func [ThresholdSign](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ext.go#L57-L61>)

```go
func ThresholdSign(expandedSecretKeyShare []byte, publicKey PublicKey, message []byte, rShare []byte, R PublicKey) []byte
```

ThresholdSign is used for creating signatures for threshold protocols that replace the values of the private key and nonce with shamir shares instead\. Because of this we must have a custom signing implementation that accepts arguments for values that cannot be derived anymore and removes the extended key generation since that should be done before the secret is shared\.

expandedSecretKeyShare and rShare must be little\-endian\.

## func [Verify](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L236>)

```go
func Verify(publicKey PublicKey, message, sig []byte) (bool, error)
```

Verify reports whether sig is a valid signature of message by publicKey\. It will panic if len\(publicKey\) is not PublicKeySize\. Previously publicKey is of type PublicKey

## type [Commitments](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L37>)

Commitments is a collection of public keys with each coefficient of a polynomial as the secret keys\.

```go
type Commitments []curves.Point
```

### func [CommitmentsFromBytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L51>)

```go
func CommitmentsFromBytes(bytes [][]byte) (Commitments, error)
```

CommitmentsFromBytes converts bytes to commitments

### func \(Commitments\) [CommitmentsToBytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L40>)

```go
func (commitments Commitments) CommitmentsToBytes() [][]byte
```

CommitmentsToBytes converts commitments to bytes

## type [KeyShare](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L26-L28>)

KeyShare represents a share of a generated key\.

```go
type KeyShare struct {
    *v1.ShamirShare
}
```

### func [KeyShareFromBytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L67>)

```go
func KeyShareFromBytes(bytes []byte) *KeyShare
```

KeyShareFromBytes converts byte array into KeyShare type

### func [NewKeyShare](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L31>)

```go
func NewKeyShare(identifier byte, secret []byte) *KeyShare
```

NewKeyShare is a KeyShare constructor\.

### func \(\*KeyShare\) [VerifyVSS](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L197>)

```go
func (share *KeyShare) VerifyVSS(commitments Commitments, config *ShareConfiguration) (bool, error)
```

VerifyVSS validates that a Share represents a solution to a Shamir polynomial in which len\(commitments\) \+ 1 solutions are required to construct the private key for the public key at commitments\[0\]\.

## type [Message](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L11>)

```go
type Message []byte
```

### func \(Message\) [String](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L13>)

```go
func (m Message) String() string
```

## type [NonceShare](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/noncegen.go#L18-L20>)

NonceShare represents a share of a generated nonce\.

```go
type NonceShare struct {
    *KeyShare
}
```

### func [NewNonceShare](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/noncegen.go#L23>)

```go
func NewNonceShare(identifier byte, secret []byte) *NonceShare
```

NewNonceShare is a NonceShare construction

### func [NonceShareFromBytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/noncegen.go#L28>)

```go
func NonceShareFromBytes(bytes []byte) *NonceShare
```

NonceShareFromBytes unmashals a NonceShare from its bytes representation

### func \(NonceShare\) [Add](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/noncegen.go#L91>)

```go
func (n NonceShare) Add(other *NonceShare) *NonceShare
```

Add returns the sum of two NonceShares\.

## type [PartialSignature](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L19-L22>)

```go
type PartialSignature struct {
    ShareIdentifier byte   // x-coordinate of which signer produced signature
    Sig             []byte // 64-byte signature: R || s
}
```

### func [NewPartialSignature](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L25>)

```go
func NewPartialSignature(identifier byte, sig []byte) *PartialSignature
```

NewPartialSignature creates a new PartialSignature

### func [TSign](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L48>)

```go
func TSign(message Message, key *KeyShare, pub PublicKey, nonce *NonceShare, noncePub PublicKey) *PartialSignature
```

TSign generates a signature that can later be aggregated with others to produce a signature valid under the provided public key and nonce pair\.

### func \(\*PartialSignature\) [Bytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L42>)

```go
func (sig *PartialSignature) Bytes() []byte
```

### func \(\*PartialSignature\) [R](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L33>)

```go
func (sig *PartialSignature) R() []byte
```

R returns the R component of the signature

### func \(\*PartialSignature\) [S](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/partialsig.go#L38>)

```go
func (sig *PartialSignature) S() []byte
```

S returns the s component of the signature

## type [PrivateKey](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L46>)

PrivateKey is the type of Ed25519 private keys\. It implements crypto\.Signer\.

```go
type PrivateKey []byte
```

### func [NewKeyFromSeed](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L111>)

```go
func NewKeyFromSeed(seed []byte) (PrivateKey, error)
```

NewKeyFromSeed calculates a private key from a seed\. It will panic if len\(seed\) is not SeedSize\. This function is provided for interoperability with RFC 8032\. RFC 8032's private keys correspond to seeds in this package\.

### func \(PrivateKey\) [Public](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L54>)

```go
func (priv PrivateKey) Public() crypto.PublicKey
```

Public returns the PublicKey corresponding to priv\.

### func \(PrivateKey\) [Seed](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L63>)

```go
func (priv PrivateKey) Seed() []byte
```

Seed returns the private key seed corresponding to priv\. It is provided for interoperability with RFC 8032\. RFC 8032's private keys correspond to seeds in this package\.

### func \(PrivateKey\) [Sign](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L74>)

```go
func (priv PrivateKey) Sign(rand io.Reader, message []byte, opts crypto.SignerOpts) (signature []byte, err error)
```

Sign signs the given message with priv\. Ed25519 performs two passes over messages to be signed and therefore cannot handle pre\-hashed messages\. Thus opts\.HashFunc\(\) must return zero to indicate the message hasn't been hashed\. This can be achieved by passing crypto\.Hash\(0\) as the value for opts\.

## type [PublicKey](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L43>)

PublicKey is the type of Ed25519 public keys\.

```go
type PublicKey []byte
```

### func [GeAdd](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ext.go#L16>)

```go
func GeAdd(a PublicKey, b PublicKey) PublicKey
```

GeAdd returns the sum of two public keys\, a and b\.

### func \(PublicKey\) [Bytes](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/ed25519.go#L49>)

```go
func (p PublicKey) Bytes() []byte
```

Bytes returns the publicKey in byte array

## type [ShareConfiguration](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/keygen.go#L77-L80>)

ShareConfiguration sets threshold and limit for the protocol

```go
type ShareConfiguration struct {
    T   int // threshold
    N   int // total shares
}
```

## type [Signature](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/sigagg.go#L16>)

```go
type Signature = []byte
```

### func [Aggregate](<https://github.com/coinbase/kryptology/blob/master/pkg/ted25519/ted25519/sigagg.go#L18>)

```go
func Aggregate(sigs []*PartialSignature, config *ShareConfiguration) (Signature, error)
```



Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
