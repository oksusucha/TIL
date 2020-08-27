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
