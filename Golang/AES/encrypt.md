### AES encrypt
```go
package encrypt

import (
	"bytes"
	"crypto/aes"
	"crypto/cipher"
	"fmt"
)

var initialVector string = "1010101010101010"

// AES 암호화 작업 key의 길이에 따라 16자리=128 bit, 32자리=256 bit
func AESEncrypt(content []byte, key []byte) ([]byte, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return nil, err
	}

	ecb := cipher.NewCBCEncrypter(block, []byte(initialVector))
	content = PKCS5Padding(content, block.BlockSize())
	encoded := make([]byte, len(content))
	ecb.CryptBlocks(encoded, content)

	return encoded, err
}

func PKCS5Padding(ciphertext []byte, blockSize int) []byte {
	padding := blockSize - len(ciphertext)%blockSize
	padedtext := bytes.Repeat([]byte{byte(padding)}, padding)
	return append(ciphertext, padedtext...)
}

func PKCS5Trimming(encrypt []byte) []byte {
	padding := encrypt[len(encrypt)-1]
	return encrypt[:len(encrypt)-int(padding)]
}
```
