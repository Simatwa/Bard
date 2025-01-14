# Bard <img src="https://www.gstatic.com/lamda/images/favicon_v1_150160cddff7f294ce30.svg" width="35px" />
Reverse engineering of Google's Gemini chatbot API, formerly **BARD**.

## Installation
```bash
 $ pip3 install --upgrade GoogleBard1
```

## Authentication
Go to https://bard.google.com/

- F12 for console
- Copy the values
  - Session: Go to Application → Cookies → `__Secure-1PSID` and `__Secure-1PSIDTS`. Copy the value of those cookie.

## Usage

```bash
$ python3 -m Bard -h
usage: Bard.py [-h] --session SESSION --session_ts SESSION_TS

options:
  -h, --help            show this help message and exit
  --session SESSION     __Secure-1PSID cookie
  --session_ts SESSION_TS
                        __secure_1psidts cookie.
```

### Quick mode
```
$ export BARD_QUICK="true"
$ export BARD__Secure_1PSID="<__Secure-1PSID>"
$ export BARD__Secure_1PSIDTS="<__Secure-1PSIDTS>"
$ python3 -m Bard
```
Environment variables can be placed in .zshrc.

Example bash shortcut:
```bash
# USAGE1: bard QUESTION
# USAGE2: echo "QUESTION" | bard
bard () {
	export BARD_QUICK=true
	export BARD__Secure_1PSID=<__Secure-1PSID>
	export BARD__Secure_1PSIDTS=<__Secure-1PSIDTS>
	python3 -m Bard "${@:-$(</dev/stdin)}" | tail -n+7
}
```

### Implementation:
```python
from os import environ
from Bard import Chatbot

Secure_1PSID = environ.get("BARD__Secure_1PSID")
Secure_1PSIDTS = environ.get("BARD__Secure_1PSIDTS")
chatbot = Chatbot(Secure_1PSID, Secure_1PSIDTS)

answer = chatbot.ask("Hello, how are you?")

print(answer['content']
```

### Async Implementation:
```python
import asyncio
from os import environ
from Bard import AsyncChatbot

Secure_1PSID = environ.get("BARD__Secure_1PSID")
Secure_1PSIDTS = environ.get("BARD__Secure_1PSIDTS")

async def main():
    chatbot = await AsyncChatbot.create(Secure_1PSID, Secure_1PSIDTS)
    response = await chatbot.ask("Hello, how are you?")
    print(response['content'])

asyncio.run(main())
```

> [!IMPORTANT]
> The cookies expire after a short period; ensure you update them as frequent as possible.

## [Developer Documentation](https://github.com/Simatwa/Bard/blob/main/DOCUMENTATION.md)

Credits:
- [discordtehe](https://github.com/discordtehe) - Derivative of his original reverse engineering
