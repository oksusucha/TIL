### 유튜브 비디오 다운로드 하기

```python
import youtube_dl

def youtube_downloader():
    try:
        ydl_opts = {
            'format'        : 'bestaudio[ext=m4a]/bestaudio',  # 가장 좋은 화질로 선택(화질을 선택하여 다운로드 가능)
            'outtmpl'       : <source_path>,  # 다운로드 경로 설정
            'cachedir'      : False,
            'writeautomaticsub': True, # 자동 생성된 자막 다운로드
            'subtitleslangs': 'ko',
            'postprocessors': [{
                'key'             : 'FFmpegExtractAudio',
                'preferredcodec'  : 'wav',
                'preferredquality': '192'}]
        }

        with youtube_dl.YoutubeDL(ydl_opts) as ydl:
            ydl.download([<video_url>])

    except Exception as e:
        print('error', e)
```
```python
def youtube_downloader_for_subprocess(self):
    # .%(ext)s 를 넣은 이유는 .wav 라고 파일명을 고정해버리면 파일의 메타데이터 안에 video/mp4 로 고정되버리는 현상이 있음
    command = "youtube-dl --extract-audio --audio-format wav -f 'bestaudio[ext=m4a]/bestaudio' -o '{0}.%(ext)s' {1}".format(path, url)

    print("Command: " + command)

    subprocess.call(command, shell=True)

```
