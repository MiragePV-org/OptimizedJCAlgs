# TWINE Cipher

## Project Status
- [x] Code is working
- [x] Code is optimized
- [ ] Code is beautified
- [x] Usage info is provided
- [x] Example is provided
- [ ] Profiling data are collected & interpreted
- [ ] Side-channel vulnerability data are collected
- [x] Diploma thesis article is written

## Briefly about Twine
Twine is a lightweight block cipher which utilises a 4x4 substitution box and a 144byte RoundKey (state). Whether you use an 80-bit or 128-bit key, it gets expanded using the substitution box into a RoundKey of the constant 144-byte length. The longer key adds more entropy to the expansion procedure and therefore more entropy to the encryption and decryption itself.

## Usage
Twine extends the Cipher class, therefore you are able to use it the same way you would use any other block cipher, such as AES.
The constructor is protected, therefore the only way to instantiate Twine is through the getInstance() method.
### Interface:
Create TwineCipher instance:
````java
public static TwineCipher getInstance(byte algorithm)

algorithm                 // either TWINE_CIPHER_80 or TWINE_CIPHER_128
return                    // the instance of Twine cipher
````
Initialize TwineCipher with key for encryption/decryption:
```` java
public void init(Key theKey, byte theMode)

theKey                    // initialized 128bit AESKey with either 80 bits or 128 bits of data
theMode                   // either Cipher.MODE_ENCRYPT or Cipher.MODE_DECRYPT
throws UNINITIALIZED_KEY  // if theKey wasn't properly initialized yet
throws ILLEGAL_VALUE      // if theKey is not a 128-bit AESKey
````
Encrypt/decrypt supported data:
````java
public short doFinal(byte[] inBuff, short inOffset, short inLength, byte[] outBuff, short outOffset)

inBuff                    // input buffer
inOffset                  // input buffer offset
inLength                  // input buffer length
outBuff                   // output buffer
outOffset                 // output buffer offset
return                    // the length of processed data (same as inLength if properly executed)
throws UNINITIALIZED_KEY  // if cipher wasn't initialized using init() method.
throws ILLEGAL_VALUE      // if inLength is not a multiple of 8 (Twine is NOPAD)
````
Get TwineCipher algorithm tag:
```` java
public byte getAlgorithm()

return  ALG_TWINE         // 19
````
Update (not supported):
```` java
public short update(byte[] inBuff, short inOffset, short inLength, byte[] outBuff, short outOffset)

throws ILLEGAL_USE        // always throws this
// this method is not supported on lightweight Twine cipher (just use doFinal)
````
Initialize with array (not supported)
```` java
public void init(Key theKey, byte theMode, byte[] bArray, short bOff, short bLen)

throws ILLEGAL_USE        // always throws this
// this method is not supported on lightweight Twine cipher (use supported init)
````

## Example
Simple example how to create, instantiate and use TwineCipher in the JavaCard applet:
```` java
// create entities
private Cipher m_twine = null; // cipher
private AESKey m_aes = null;   // key

// instantiate the 128-bit cipher and key
m_twine = TwineCipher.getInstance(TwineCipher.TWINE_CIPHER_128);
m_aes   = (AESKey) KeyBuilder.buildKey(KeyBuilder.TYPE_AES, KeyBuilder.LENGTH_AES_128, false);
// (note that instatiating with 80-bit key still requires 128-bit AESKey)

// init des key and twine cipher for encrpytion
m_aes.setKey(m_ramArray, (short) 0);
m_twine.init(m_aes, Cipher.MODE_ENCRYPT);

// encrypt 32 bytes of data
short ret = m_twine.doFinal(m_ramArray1, (short) 0, (short) 32, apdubuf, ISO7816.OFFSET_CDATA);
````

## Optimizations used
* DESKey changed to AESKey (no impact on performance, but is more true to the nature of Twine)
* Removed redundant arrays (RAM saving)
* Inlined single-use private methods (faster)
* Changed visibility of non-interface methods to package-only (safer)
* temporary variables are now all stored in one array (faster, but not always)
* Minimized number of method parameters (worse readability but faster)
* Code beautification (better readability) (not complete)

## Performance measurement results