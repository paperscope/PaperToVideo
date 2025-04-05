# PaperToVideo

探索由 Paper 生成介绍视频的 AI 工作流



## 部分提示词

```text


## 任务一 HTML格式 PPT 生成


根据 md 生成一个直播使用的科技风 HTML 文件格式的 PPT，PPT 语言为中文，专业名词、姓名等不用翻译，需要支持左右键翻页功能，首页中英文标题且需要有单位。

每页PPT的布局，让文字具有更好的设计感和区域感：

PPT 中需要包含一些 md 中关键图片，需要渲染的图片直接用 md 中的链接



## 任务二： PPT 生成视频

请使用 Puppeteer 给每页 PPT 截一个图保存到本地，并使用硅基流动的 API 给每页 PPT 生成一段语音 mp3 到本地，最后使用 ffmpeg 将图片和音频合成视频。


以下为硅基流动 tts 的 python 使用方法，实际运行时使用 nodejs 实现。

```python

from pathlib import Path
from openai import OpenAI

speech_file_path = Path(__file__).parent / "siliconcloud-generated-speech.mp3"

client = OpenAI(
    api_key="APIKEY 可以从环境变量 SiliKey 中获取", # 从 https://cloud.siliconflow.cn/account/ak 获取
    base_url="https://api.siliconflow.cn/v1"
)

with client.audio.speech.with_streaming_response.create(
  model="FunAudioLLM/CosyVoice2-0.5B", # 支持 fishaudio / GPT-SoVITS / CosyVoice2-0.5B 系列模型
  voice="FunAudioLLM/CosyVoice2-0.5B:bella", # 系统预置音色
  # 用户输入信息
  input="你能用高兴的情感说吗？<|endofprompt|>今天真是太开心了，马上要放假了！I'm so happy, Spring Festival is coming!",
  response_format="mp3" # 支持 mp3, wav, pcm, opus 格式
) as response:
    response.stream_to_file(speech_file_path)



```

```
