### 업로드되는 파일 전송 받기
```proto
service YOUR_SERVICE_NAME {
    rpc UploadFile(stream YOUR_REQUEST) returns (YOUR_RESPONSE) {};
}

message YOUR_REQUEST {
    oneof data {
        FileInfo metadata = 1;
        bytes    chunk_data = 2;
    }
}

message YOUR_RESPONSE {
    // SPEC
}

message FileInfo {
    // SPEC
}
```

### Server side
```go
func (server *FileServer) UploadFile(stream pb.YOUR_SERVICE_NAME_UploadFileServer) error {
	fmt.Println("UploadFile start")

	// receive file metadata
	req, err := stream.Recv()
	if err != nil {
		fmt.Println("Recv : ", err)
		return status.Errorf(codes.Unknown, "cannot receive file info : %v", err)
	}

	fmt.Printf("Requests : " + req.GetMetadata())

	// receive chunk data
	fileData := bytes.Buffer{}
	fileSize := 0

	for {
		if err := stream.Context(); err.Err() != nil {
			//fmt.Println("stream.Context : ", err)
			return err.Err()
		}

		//fmt.Println("waiting to receive more data")

		req, err := stream.Recv()
		if err == io.EOF {
			//fmt.Println("no more data")
			break
		}
		if err != nil {
			//fmt.Println("stream.Recv : ", err)
			return err
		}

		chunk := req.GetChunkData()
		size := len(chunk)

		//fmt.Println("received a chunk with size : ", size)

		fileSize += size

		_, err = fileData.Write(chunk)
		if err != nil {
			//fmt.Println("fileData.Write : ", err)
			return err
		}
	}
	fmt.Println("Received file(s)...")

	if err := stream.SendAndClose(res); err != nil {
		fmt.Println("SendAndClose : ", err)
        
        return &pb.YOUR_RESPONSE{
            // fill your response spec in
        }
	}

	fmt.Println("UploadFile end.")
  
	return &pb.YOUR_RESPONSE{
		// fill your response spec in
	}
}
```

### Client side
```go
const (
	// ChunkSize define buffer size when send to server
	ChunkSize = 2048
)

func main() {
	// fmt.Println("start : ", time.Now())
	transportOption := grpc.WithInsecure()
	client, err := grpc.Dial("IP:PORT", transportOption)
	if err != nil {
		fmt.Println("Dial : ", err.Error())
		return
	}
	defer client.Close()

	// NewYOUR_SERVICE_NAMEClient create a new YOUR_SERVICE_NAMEClient interface
	service := pb.NewYOUR_SERVICE_NAMEClient(client)

	// UploadFile create a new FileService_UploadFileClient interface
	stream, err := service.UploadFile(context.Background())
	if err != nil {
		fmt.Println("UploadFile : ", err.Error())
		return
	}

	// create Metadata
	req := &pb.YOUR_REQUEST{
        Data: &pb.YOUR_REQUEST_Metadata{
			Metadata: // Fill YOUR_SPEC in,
		},
	}

	// Send which is YOUR_SERVICE_NAME_UploadFileClient interface send first message of oneof to server
	if err := stream.Send(req); err != nil {
		fmt.Println("Send : ", err)
		return
	}

	// 파일 열기
    fiePath := "/YOUR_TARGET_FILE"
	file, err := os.Open(fiePath)
    defer file.Close()
	if err != nil {
		fmt.Println("Open : ", err)
		return
	}

	// 파일 읽기
	reader := bufio.NewReader(file)
	buffer := make([]byte, ChunkSize)
	size := 0

	// chunk_data 보내기
	for {
		read, err := reader.Read(buffer)
		if err == io.EOF {
			fmt.Println("file is end")
			break
		}

		size += read

		req := &pb.YOUR_REQUEST{
            Data: &pb.YOUR_REQUEST_ChunkData{
				ChunkData: buffer[:read],
			},
		}

		if err = stream.Send(req); err != nil {
			fmt.Println("Send : ", err)
			break
		}
	}

	res, err := stream.CloseAndRecv()
	if err != nil {
		fmt.Println("CloseAndRecv : ", err)
		return
	}
}
```
