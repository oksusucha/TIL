### File 암호화 하기

```go
// file AES 암호화
func FileEncodingForAES(originFilePath string) (key string, outputPath string, err error) {
	var file []byte
	var encodedFileByte []byte

	file, err = ioutil.ReadFile(originFilePath)
	if err != nil {
		return "", "", err
	}

	key = keyString(32)

	encodedFileByte, err = AESEncrypt(file, []byte(key))
	if err != nil {
		return "", "", err
	}

	encodedBase64 := tools.Base64Encoding(encodedFileByte)

	savePath := "/tmp/encrypt/test"
  
  if _, err := os.Stat(savePath); os.IsNotExist(err) {
		os.MkdirAll(savePath, 0755)
	}

	outputFilePath = fmt.Sprintf("%s/%s", savePath, "test.txt")

	f, err := os.Create(outputFilePath)
	defer f.Close()

	if err != nil {
		return "", "", err
	}
	f.Write([]byte(encodedBase64))

	return key, outputFilePath, nil
}
```
