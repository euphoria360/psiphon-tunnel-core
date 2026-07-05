[![CI-Build-Pipeline](https://github.com/shirokhorshid/psiphon-tunnel-core/actions/workflows/build.yml/badge.svg)](https://github.com/shirokhorshid/psiphon-tunnel-core/actions/workflows/build.yml)

Psiphon Tunnel Core (Console Client Fork)
================================================================================

Overview
--------------------------------------------------------------------------------

This repository is a specialized fork of the Psiphon Tunnel Core project, focusing specifically on cross-compiling, configuring, and optimizing the standalone command-line **Console Client** (`ConsoleClient`). 

Unlike the upstream repository, this project includes:
- **Automated Multi-Architecture Builds:** GitHub Actions workflows compiled automatically for Linux, Windows, and macOS architectures on every upstream change.
- **Advanced Domain Fronting Configuration:** Pre-optimized parameters specifically tuned for restrictive networking environments utilizing Domain Fronting, `FRONTED-MEEK` variants, and `IndistinguishableTLS`.
- **System Automation:** Native systemd service wrappers for headless deployment on home servers and remote virtual private servers (VPS).

---

Automated Pre-Compiled Releases
--------------------------------------------------------------------------------

You do not need to compile this project manually. Automated CI/CD pipelines run every 24 hours to monitor changes, sync with upstream modifications, compile optimizations, and publish compressed binaries.

### Supported Architectures
Navigate to the **Releases** tab of this repository to download pre-packed assets:
* **Linux:** `amd64` (64-bit), `386` (32-bit), `armv7` (Raspberry Pi/ARM), `arm64` (Modern AArch64), `mipsle` (Routers/Embedded)
* **Windows:** `amd64`, `386`
* **macOS:** `amd64` (Intel), `arm64` (Apple Silicon M1/M2/M3/M4)

---

Manual Compilation Guide
--------------------------------------------------------------------------------

If you prefer to build the binaries locally from source, ensure you have **Go (Golang) v1.21 or newer** installed on your system.

### 1. Clone the Fork
```bash
git clone https://github.com/shirokhorshid/psiphon-tunnel-core.git
cd psiphon-tunnel-core/ConsoleClient
```
### 2. Native System Compilation
To compile the console client executable for your current host operating system and CPU architecture, run:
```Bash
CGO_ENABLED=0 go build -v -ldflags="-s -w" -o psiphon-tunnel-core .
CGO_ENABLED=0: Forces a static binary compilation, eliminating runtime dependencies on shared C libraries (maximizes portability across different Linux distributions).
```
`-ldflags="-s -w"`: Strips debugging information and symbols from the binary, shrinking the executable footprint significantly.

### 3. Cross-Compilation Matrix
Go makes it trivial to cross-compile for other hardware frameworks directly from your terminal.

#### Compile for Linux ARM64 (e.g., Raspberry Pi, Oracle Cloud ARM):
```Bash
env GOOS=linux GOARCH=arm64 CGO_ENABLED=0 go build -v -ldflags="-s -w" -o psiphon-tunnel-core-linux-arm64 .
```
#### Compile for Linux MIPSLE (Embedded Router Boards):
```Bash
env GOOS=linux GOARCH=mipsle CGO_ENABLED=0 go build -v -ldflags="-s -w" -o psiphon-tunnel-core-linux-mipsle .
```
#### Compile for Windows 64-bit:
```Bash
env GOOS=windows GOARCH=amd64 CGO_ENABLED=0 go build -v -ldflags="-s -w" -o psiphon-tunnel-core.exe .
```

---

Configuration File Layout (shirokhorshid.json)
-------------------------------------
This fork is designed to interface with secure, multi-layered configurations. Below is an exhaustive reference layout for shirokhorshid.json. This format drops standard upstream un-obfuscated HTTP fallbacks and strictly enforces strict Domain Fronting (FRONTED-MEEK-*) via targeted Content Delivery Networks (CDNs) alongside cryptographic handshakes.

Create a configuration file named config.json and adjust paths as necessary:

```JSON
{
    "AdditionalParameters": "5smXlYWkEvA2VgXbgcTzLR8xgC8dpwB+xEi0GuTbfpznFgUq1QoND3NSbvZgXR4glfOHAjW8xs7gGPElcz44nd/0zXX8D/FETOvjX2cTGfHITpCo/WvfVuLqCBrtjtFG00vWUy+uBqJe2s0HNNs1bxUVj1KBROpwOV9qpf1zmz2SdHzKTs91ENcA+n9MDd9su+pyTPCbbxR9j6uGNge+R24XiIkLJUszoNVKL8ug19bO89unT/tosfI49c3XVQuCSnlFlC6/1FVgtBmJ230vKjJONvlPG1u2Pl8DDKD6ozurV8KJDTRRWYxe4aJyu8iD01tpbMbNLSgTQgixT+LWoFKMRSVtQJ+jFKiD5jvyPndeUcHcKUzYGgq6heP/mdLoZGUjLCW1j+wuloGnYHeeVY9958ulV+iKNdFUlzvZEqELWdyLf48t/OvxLGFmASMyyhdUoYpzsEHatTUMbOAojmHZJsQ5tAiaKQd4UW8IjfUEXMur+oy+m/aZ/n7B8FIZPR/P8B1vEgJRPDAt+uieLEVdOfR8E4tK1SsQhTVZhSAwOyKqhfjIKP9f2Tz19IniSNkJ9ox8fZY8kZ3I1tGnyO600Z5L0/JQi/pYrCRnYl13wmfRL17+fouWFrU4r9olUu/FBWCG5r7/Bc7hA4KEC9+ZvKNAEnGdR+WvFVW2zQ3mV0AfVYqkF93SSyNQQrlJ1YusjZE30LLn8eL7tjWaltVx/mtIViPsM4LWoTyqY8Me1ZWqCpSa+Z69LmUbVYLXFy6uyTEq4i/Zf6RAWfM/5nrGF8rAcdvitrqh+KZ1KTjc8sQcG1OAM5x0Q8Dr/Zdf3Gf8/j9LlKUaKM6+muTeUbq/GFOV9OLWI8A4aSRSIGVnJVlYo5Dj26wDQXONrGW76gy4LT3QoEXNK8TExIish1k3HHEVDcmszwZX13aCEMpH7i1GTjvv1XjRJwMpmmXHfzIUVaH2eBB4NBBi8hUci0zBdObZUCbyASTlQ87q1MugX7L8iR/tYUNUNcpD4Uo2cL5rKXRJRYMA07uHBZPjR84vjP88qW3wZD5jK+A5565gMosbYSZVKaI12QsXTOn+emQ7oPLyDKQLgnXywP3XCG/H0FdgV1ZfoxgxRv7LEAhyb0P7MtX02gVnJVP2SHqIU4ZIiZpUKjk4x6Xvu+BBgTiX+52PEDybc2D5BB49BK1Ky29qnUyTnf3GhfTaELQs29sgIoBce+gSGO6NL5qZXVTSrVDfoFumGmXxylYGrvk6FE5Z0pO3Io/3Aof1Cy+vU9YvYOD9/JzfcBA3h8U+WzJS7D0V7gt2uANWlD0420/PTXrrRt1EnW1AdAQPy3q3GZJK++GkuTv6yF/de7sk/wsryr6febtLLobaOsLuZlXY6ARLg2s4i8JN0VBHaHEs2WFlBZVd9Ea0UQUlcS9jeX7AbAIOLS2jYX0IR+aYdN77OULM94GLjkDFDidaer4wJ/lPElwR0Enf54Or8D+G66ChvkMXA45wojT95NHV7j4+NnE34+fDsyX6FxAFfI1EuQTi/aLc+rnHwbCboifWVMRpt/Wg5UVXhn2PSfxUPxzHIZaANg9kXzurSnVZzUmm3iO7FjKlbf+1JOSl6TgXOfqcowbfbBpBr5jQK0HoqrAcIgab53mgDR22w6RvRxBoR84OPf68ybXdzl17DMtmL3Qz+QB0agXhg9QW+VN6o0fkqtQjxKTzflel4hLpjCsiQkTwG9eEyHDKQv6Xa3Grr52EYtZuo6OK4pEQ9IfzN4zl9cAdmWWoSQAgJqsuzTaHjIQioNec+opdQx0Nndk6IMWojL4c+5SW6Vx3ib8z4PxmU3AN3djXh8RN6JpVgRC3jr269HwyIbIQIZywsdpbA+LYfuwaD+/cDeDGCsfipy7fAxkVxSlyYikDW+oIceJNxQIMN2sngbmBzdKBAboQo632BDc+sKfl389FhB0IbEXfLinvOAAeibqwVLHCB3lr5snCNaGurO6SxZYqb+ppco/bdnElDr2v8LA7/q5rUAOvFxSdo3fOax4yXjWbj9nht7XZZRYrm0kM/daNee9LU05ZHFkxb3E9aic7uKEXosrUxnnxYxU7dgD3+wdkEQmkABT4UuOd//w77cjpyjMNAwhpCtCJy7yBupGy/GlVrBqLogu/sN9MDChaQLy14s6VrcRIRxGCwIjCP9j4G2DVHIYuSwGg/x7CZ23ENvi8nPRLbv5nJqfdPws6cT8GJd4jSBdMKfTFwrLk4pgikGzNfzyVgFMH264O9UBh51B6P7zP8BhjB/54DcVMZPLCBi5STnELsCs6ZSzL2vsHWy+Dej5YamHADPiE1JbYKT26GK2sdVX9aUI2Vu3s9aqQJhmz+1drce/GwJQoyjQy/gd1MMdp9MnhEOXPXzEjcP33G80pfrguOdwaz43/8N+pMBXYkRDbhg/Tnce/zqDXkfVgQiiclX4HZVHm1iL1Bix3bQEoUCRUnIjpBimz1gbJOaFyWh9sP9D8x0L7F5twTOXYAIz0mMpOogn0KTKr7CSjnFZzFM6OrxvZkwjHbF0TGD5kF+eV/ocDi44/vN3LJsVH2EFJOn4cYZkZBwvTPA7SmnxunH5kLg5hxiFUDNNpop5r1+1SekygNLCRADjccltz1qilGQktyfIYr+vNTFjPVt8iK95GtcS0aiGAP075fpvNgSKglV85gKBhLNuNqUfJXJgmh1Vea5Hx9EmFC+mCyrh/u0dxuxRA7ezEtwu87De2PM/mfJ1OxVeJLy1KntKzCmaz0UTjdlWDZ2DsYMgIet94WnB5WytSsREdxR83oGeXm8FR20TNJ76WDCBb9ydNqfd3AWGN999+HkjMQkIdXoVY2PydDpM573WbH9aexMjnSUa9yBDAf4xdLqJb7eu5ltQPZ+3d2vYE8vHeF/2LuZ6BhJSK84qx5KKZajN/jm86NdDOcieWlmgStInPKDhVzlJzdDCYDJnlYc7a+Yhl/ObzDECIgsAQoP8TRX6GBUTS7b+FnpbKTQ9X6MMqaZqnsruXVatLLWIeZiIXBhEZ48+zSAF5bi+yDpH0nuxg+f9WwsIk5HTiDla4fetz4mmAPqaq6iw8eQ8a6acpZNXbFl057XmJZLhdwvoVMsr2W/SniZR4jsKJRjSDjedh2IdoEw4Mc59oLzms73GLGnoZOibbv0mP3NEB50oBn40x3reLvD2tdqEYE9DhSrhVWtyVc0wLaz1PxvgDL0Q4riISrtjDiCHFbq2OdRsFkg+EEG8LdgglzkNo+Gn80Q/pw3Jm+NoosxSHlLD/U/9ylCrkT94+8E3zC/wTgh9D31LtNfUI+N5aOJ/ax9oXwZ/3eM8P+rXqoHO3/I8oHuBj6YVseDUw/m5CARoCp68gPFQtN3zqOdePpbS7nvTY4dft5Gnxo2wXgL3PcxRQijnoEbgkkC0QMN1+kUJXIPdSInQc20H9TRM+KX8x+y68rIh6plzZkH8/arDNHxjSCSGyTounXnOXNjfoKMlnVtZEHW3URFjtdudnR9NeIrc8fIRmpmx23lsJTTWoLsLGIhStEhUtSkhZ7azFRgRnmA/A5uAcVY4tqQM7CvNoYXlGWaKDhGMrUqKsEc7iLRZ/3RZ8VO7Wh41uvdTJtP6DSpoHiJKy9XVDDlLVO6SR6RqFzwh4mlIxxr56jKWBUu2Ma4yYe/CUM/U2Xm4EEYnQqV+3UUbairTSPyia+U/iUqPZQ0PFGMv1C03zv1Bx7moZMZZp4Rga4hmZO37h2tTD89fqLhPV0KlV0ieiNUOkfFuTZDsUM65kzqKXZBbYFrkMEGrqvFb5MDzHeRWjJF5pNUAS5qYtXLUqFDdK9P0bgQZb0FXv4bArnf+bxvFE4M8xNCbWcN5BBWqCymnkd7mjd3B79oFBkHzbCQTTBJdZUj91pwcv4e1jXFYD9eIIee8N1XMvkAyUroxxUl5cS3u9zDash84yLRAqGaAAQJG7eZbVXovPxLWk+In/uLVOOEESIY4TaO8XJcV2zgaW56e0s8dD04pvrkuT21DKudxdASmS2dSMOtQIpvE2qtJT5MZ7h2vTvusluX4u8Z0Zed2F5W3GU3Pv8hbNIpV06zD3XT5BEgvEZPICQoCWN6J0XeBVg2kU6qDnaaIoKvqevfHxn0HklNDyjWvjGH1yOyz+u48UhTask+gr6K4l0uhosxf3GAYmWbPJQz94LoJ/2LR6KEj8xb4mxyvs7GbqqmllzfJMhWPmg386vse+2OwAJpmgpnT+zxX7usU7UZitV0QnJgKZs37XfhS2Xla7bBnME4UEMVzqfonax2H/I61kQhACHE8d2Gp4FMkxfBi61XonVFCHM7C7MApadbm4UHfir7xeWwatHX9OethpbqZQwC7NNCZ5vBOch2Bzj3nurGBdycd3kgG70iQwR3QKPMfDgoc9+dy2n4WBhdwSABSwuu3/YyADKtZ+Bz91hYOYj/586quiUklGpNPRC2+ZTgfWB7uzR2/+0lmTICAjb6q9meKwYUHXCZskv7vJwtD5bXPhf/QkE5/tqM4Wifr4bj/Oia+mpMOZr6Zv0t86tCGq5zhP2xY61FjuOzrMayMQYtaH+jkjjmRfuV/+jyGsBOl9FoOH6n07gJ6xTROpo3zcZ9xJ690vctLfnmM8Tnuy1PMuiGc+QhPcYTyDKRISdoIMAncQ6c1u1A+5ZvTCFltCK1Z3fCzr4s2M576WDCBb9ydNqfd3AWGN999+HkjMQkIdXoVY2PydDpM573WbH9aexMjnSUa9yBDAf4xdLqJb7eu5ltQPZ+3d2vYE8vHeF/2LuZ6BhJSK84qx5KKZajN/jm86NdDOcieWlmgStInPKDhVzlJzdDCYDJnlYc7a+Yhl/ObzDECIgsAQoP8TRX6GBUTS7b+FnpbKTQ9X6MMqaZqnsruXVatLLWIeZiIXBhEZ48+zSAF5bi+yDpH0nuxg+f9WwsIk5HTiDla4fetz4mmAPqaq6iw8eQ8a6acpZNXbFl057XmJZLhdwvoVMsr2W/SniZR4jsKJRjSDjedh2IdoEw4Mc59oLzms73GLGnoZOibbv0mP3NEB50oBn40x3reLvD2tdqEYE9DhSrhVWtyVc0wLaz1PxvgDL0Q4riISrtjDiCHFbq2OdRsFkg+EEG8LdgglzkNo+Gn80Q/pw3Jm+NoosxSHlLD/U/9ylCrkT94+8E3zC/wTgh9D31LtNfUI+N5aOJ/ax9oXwZ/3eM8P+rXqoHO3/I8oHuBj6YVseDUw/m5CARoCp68gPFQtN3zqOdePpbS7nvTY4dft5Gnxo2wXgL3PcxRQijnoEbgkkC0QMN1+kUJXIPdSInQc20H9TRM+KX8x+y68rIh6plzZkH8/arDNHxjSCSGyTounXnOXNjfoKMlnVtZEHW3URFjtdudnR9NeIrc8fIRmpmx23lsJTTWoLsLGIhStEhUtSkhZ7azFRgRnmA/A5uAcVY4tqQM7CvNoYXlGWaKDhGMrUqKsEc7iLRZ/3RZ8VO7Wh41uvdTJtP6DSpoHiJKy9XVDDlLVO6SR6RqFzwh4mlIxxr56jKWBUu2Ma4yYe/CUM/U2Xm4EEYnQqV+3UUbairTSPyia+U/iUqPZQ0PFGMv1C03zv1Bx7moZMZZp4Rga4hmZO37h2tTD89fqLhPV0KlV0ieiNUOkfFuTZDsUM65kzqKXZBbYFrkMEGrqvFb5MDzHeRWjJF5pNUAS5qYtXLUqFDdK9P0bgQZb0FXv4bArnf+bxvFE4M8xNCbWcN5BBWqCymnkd7mjd3B79oFBkHzbCQTTBJdZUj91pwcv4e1jXFYD9eIIee8N1XMvkAyUroxxUl5cS3u9zDash84yLRAqGaAAQJG7eZbVXovPxLWk+In/uLVOOEESIY4TaO8XJcV2zM0xceaxbF8S3EU9zQtY7Nf2J/osehIdZW4fjIGPd/eLMIlcM5q+q7alRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GpGSmC0uZeJAyqtm3+UygMe1a4oc5VrlTkFjRi0Z9FDSbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8GSTbVUBXPAlVUD+2GedrZqMB3tetFyLYkOCY0e0Gl1PfNMXEmsWxfEtxFPc0LWOzX9ifzrHoSHWVuH4yBj3f3izCJXDOavqu2pRrOHluVIYnKXFqAVWuKe1Wmnz6u1VIwg5Dg8HA1Wa4Ov8LdpmnR+hs7VKqXX6dMGqZnhryaeQCjtXYxOHujZJwE7OGFbkEH08jjJEbqG34IwfiZV3fUA5Q7e6vN5X+SzSYAr8VKI4CScGkJScU85DV3zirvrQ4QWK9xXoZQDChS6efQi3qVT1W3z31GSWlE1ull7v0zXuq7gN2Mt8Bk8xQUL+Um7trWU2e68k0lKAaPQYIrd87ERtOTQn0VZ2fx/tOaOgIjc54tFXOw7UDnnMXhaNH6lk8Pmqjci/T1m6/h9h+B4HpaBOo7tFTrJEbZUjac2SueLrSdfEz+IcDVAHXd9DfMy83Adx41so7zQmekxTP8G",
    "Authorizations": [],
    "ClientPlatform": "Windows_10.0.19044_11",
    "ClientVersion": "187",
    "DataRootDirectory": "/opt/psiphon/data",
    "DeviceRegion": "IR",
    "EgressRegion": "",
    "EmitDiagnosticNetworkParameters": false,
    "EmitDiagnosticNotices": false,
    "EmitServerAlerts": false,
    "EnableFeedbackUpload": true,
    "EnableUpgradeDownload": false,
    "LogLevel": "info",
    "DisableLocalHTTPProxy": true,
    "LocalSocksProxyPort": 22222,
    "MigrateDataStoreDirectory": "",
    "MigrateObfuscatedServerListDownloadDirectory": "",
    "MigrateRemoteServerListDownloadFilename": "",
    "MigrateUpgradeDownloadFilename": "",
    "NetworkID": "949F2E962ED7A9165B81E977A3B4758B",
    "PropagationChannelId": "92AACC5BABE0944C",
    "SponsorId": "1BC527D3D09985CF",
    "UpstreamProxyUrl": "",
    "UseIndistinguishableTLS": true,
    "FeedbackEncryptionPublicKey": "MIICIDANBgkqhkiG9w0BAQEFAAOCAg0AMIICCAKCAgEAxltZsddAqX0qE4dK+X7QfcPfoGbvs6DAxkwY5Cb7mcWW9YpNesOdb+aq0kmDEeUDnMYIVEnUnNLFSOpF/CvjvZ1WQpxYy2sE/ulQUXO9XCtoucM4jaZIKza9TPNUsWaiiMC86UOO6kjZLRodosXwgdykfbn0GGOy90urkMTygSi1JmnUjDqHXNr8mgVS/9qTMX68N598CjzU3zeBJi5Rh2wChRzMDw7y0umJ/xJ7vevJOmEp5qGg4J4x5hMAagG1AF4SDgXwVdSKcwcRoeUUxmmWRgyirPJdEgyLCFNX0Z1LhWmB+Kz8aq7+d+5eEIoIxjmRu4O9AfB/ngvNwapIBSDj/STPszsluH2lIGY9nDBVxKZqQ8oGjDQMoFedzRu2z2dkVYqCoN7Lfve3JTku1MxsIylh67+9emW9d1F+I5v+LWicBrusmRStSB7z4CUuqZEa8GYKNGM0A1Axkdz9y/Dv+4NB6cEw01szEXl/9hJxKh8MStLCFZ1eNaokrcbxnPGHDdZSqGmV3x8eRy1GvQv0xRnoRpyaqdmQvjb7XupiTD+5GT+7PjAwXTN4kJtm39DyIEwKmmXWcFQtn6JWefwRbcXwKKDjj99QssyYQp+7EPiv/QwUAMnHTN2CrWYXEhbBHBkdgxkioPJ47j8nXxydXPBKXcnCruQ4ICEmrsECAQM=",
    "FeedbackUploadURLs": [
        { "OnlyAfterAttempts": 0, "SkipVerify": false, "URL": "aHR0cHM6Ly9zMy5hbWF6b25hd3MuY29tL3BzaXVwbG9hZC8=" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuZmluYW5jaWFsY29sbGVnZXBvaW50d3JhcC5jb20uZ2xvYmFsLnByb2QuZmFzdGx5Lm5ldC8=" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuaGlkZGVuc3Rvcmllc2NvZGVzcGxhbm5lci5jb20uZ2xvYmFsLnByb2QuZmFzdGx5Lm5ldC8=" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuZ3JlZWtsb3R0b3NhZmFyaXdlc3QuY29tLmdsb2JhbC5wcm9kLmZhc3RseS5uZXQv" }
    ],
    "ObfuscatedServerListRootURLs": [
        { "OnlyAfterAttempts": 0, "SkipVerify": false, "URL": "aHR0cHM6Ly9zMy5hbWF6b25hd3MuY29tL3BzaXBob24vd2ViL21qcjQtcDIzci1wdXdsL29zbA==" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuaGVyYm14ZGlpbmNvcnBvcmF0ZWQuY29tL3dlYi9tanI0LXAyM3ItcHV3bC9vc2w=" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuY29ycG9yYXRlaGlyZXByZXNzdGguY29tL3dlYi9tanI0LXAyM3ItcHV3bC9vc2w=" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuZXVyb3BlYW5wYXJzbG9nb2tpY2suY29tL3dlYi9tanI0LXAyM3ItcHV3bC9vc2w=" }
    ],
    "RemoteServerListSignaturePublicKey": "MIICIDANBgkqhkiG9w0BAQEFAAOCAg0AMIICCAKCAgEAt7Ls+/39r+T6zNW7GiVpJfzq/xvL9SBH5rIFnk0RXYEYavax3WS6HOD35eTAqn8AniOwiH+DOkvgSKF2caqk/y1dfq47Pdymtwzp9ikpB1C5OfAysXzBiwVJlCdajBKvBZDerV1cMvRzCKvKwRmvDmHgphQQ7WfXIGbRbmmk6opMBh3roE42KcotLFtqp0RRwLtcBRNtCdsrVsjiI1Lqz/lH+T61sGjSjQ3CHMuZYSQJZo/KrvzgQXpkaCTdbObxHqb6/+i1qaVOfEsvjoiyzTxJADvSytVtcTjijhPEV6XskJVHE1Zgl+7rATr/pDQkw6DPCNBS1+Y6fy7GstZALQXwEDN/qhQI9kWkHijT8ns+i1vGg00Mk/6J75arLhqcodWsdeG/M/moWgqQAnlZAGVtJI1OgeF5fsPpXu4kctOfuZlGjVZXQNW34aOzm8r8S0eVZitPlbhcPiR4gT/aSMz/wd8lZlzZYsje/Jr8u/YtlwjjreZrGRmG8KMOzukV3lLmMppXFMvl4bxv6YFEmIuTsOhbLTwFgh7KYNjodLj/LsqRVfwz31PgWQFTEPICV7GCvgVlPRxnofqKSjgTWI4mxDhBpVcATvaoBl1L/6WLbFvBsoAUBItWwctO2xalKxF5szhGm8lccoc5MZr8kfE0uxMgsxz4er68iCID+rsCAQM=",
    "RemoteServerListURLs": [
        { "OnlyAfterAttempts": 0, "SkipVerify": false, "URL": "aHR0cHM6Ly9zMy5hbWF6b25hd3MuY29tL3BzaXBob24vd2ViL21qcjQtcDIzci1wdXdsL3NlcnZlcl9saXN0X2NvbXByZXNzZWQ=" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuaGVyYm14ZGlpbmNvcnBvcmF0ZWQuY29tL3dlYi9tanI0LXAyM3ItcHV3bC9zZXJ2ZXJfbGlzdF9jb21wcmVzc2Vk" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuY29ycG9yYXRlaGlyZXByZXNzdGguY29tL3dlYi9tanI0LXAyM3ItcHV3bC9zZXJ2ZXJfbGlzdF9jb21wcmVzc2Vk" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuZXVyb3BlYW5wYXJzbG9nb2tpY2suY29tL3dlYi9tanI0LXAyM3ItcHV3bC9zZXJ2ZXJfbGlzdF9jb21wcmVzc2Vk" }
    ],
    "ServerEntrySignaturePublicKey": "sHuUVTWaRyh5pZwy4UguSgkwmBe0EHtJJkoF5WrxmvA=",
    "UpgradeDownloadClientVersionHeader": "x-amz-meta-psiphon-client-version",
    "UpgradeDownloadURLs": [
        { "OnlyAfterAttempts": 0, "SkipVerify": false, "URL": "aHR0cHM6Ly9zMy5hbWF6b25hd3MuY29tL3BzaXBob24vd2ViL21qcjQtcDIzci1wdXdsL3BzaXBob24zLmV4ZS51cGdyYWRl" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuaGVyYm14ZGlpbmNvcnBvcmF0ZWQuY29tL3dlYi9tanI0LXAyM3ItcHV3bA==" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuY29ycG9yYXRlaGlyZXByZXNzdGguY29tL3dlYi9tanI0LXAyM3ItcHV3bA==" },
        { "OnlyAfterAttempts": 2, "SkipVerify": true, "URL": "aHR0cHM6Ly93d3cuZXVyb3BlYW5wYXJzbG9nb2tpY2suY29tL3dlYi9tanI0LXAyM3ItcHV3bA==" }
    ],
    "LimitTunnelProtocols": ["FRONTED-MEEK-OSSH","FRONTED-MEEK-HTTP-OSSH","FRONTED-MEEK-QUIC-OSSH"],
    "AggressiveEstablishment": true,
    "FrontedMeekCDNScanUseBuiltInSpec": true
}
```
### Key Behavioral Settings Overrides:
`"DisableLocalHTTPProxy": true`: Standard local cleartext downstream HTTP proxy bindings are terminated to isolate processing directly to secure socks tunnels.

`"LocalSocksProxyPort": 22222`: The daemon provisions an encrypted inbound endpoint locally listening explicitly on SOCKS port 22222.

`"LimitTunnelProtocols"`: Restricts execution explicitly to targeted configurations: FRONTED-MEEK-OSSH, FRONTED-MEEK-HTTP-OSSH, and FRONTED-MEEK-QUIC-OSSH.

`"UseIndistinguishableTLS": true`: Randomizes outward client hellos and extensions across upstream routing pipelines to circumvent deeper signature-matching engines.

`"DataRootDirectory": "/opt/psiphon/data"`: Directs core state databases and certificates to mount onto paths matching stable, system-level file hierarchies.

---

Execution Guide
------------------------------------------------
### 1. File Setup
Ensure the target binary has structural read/write isolation and storage directories are generated properly:

```Bash
sudo mkdir -p /opt/psiphon/data
sudo cp psiphon-tunnel-core /usr/local/bin/psiphon-tunnel-core
sudo chmod +x /usr/local/bin/psiphon-tunnel-core
```
Place your modified configuration JSON structure into `/opt/psiphon/config.json`.

### 2. Manual Command Line Bootup
To run the client explicitly inside a standard interactive terminal shell session, execute:

```Bash
/usr/local/bin/psiphon-tunnel-core -config /opt/psiphon/config.json
```
Once connected, standard execution will stream diagnostic logs directly through `stderr/stdout`, verifying tunnel stabilization on SOCKS listener port `22222`.

---

Systemd Daemon Deployment (Auto Startup)
---------------------------------------
To turn the Console Client into a robust, headless service that automatically runs in the background on startup and restarts itself if it crashes, use systemd.

### 1. Create the Service File
Generate a configuration block inside system definitions:
```Bash
sudo nano /etc/systemd/system/psiphon.service
```
Paste the following system properties directly into the editor:

```Ini, TOML
[Unit]
Description=Psiphon Tunnel Core Background Client
After=network.target network-online.target
Wants=network-online.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/psiphon
ExecStart=/usr/local/bin/psiphon-tunnel-core -config /opt/psiphon/config.json
Restart=always
RestartSec=5s

# Security sandboxing overrides
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

### 2. Enable and Start the Core Daemon
Reload systemd configurations, enable the daemon to survive host restarts, and trigger execution immediately:

```Bash
sudo systemctl daemon-reload
sudo systemctl enable psiphon.service
sudo systemctl start psiphon.service
```

### 3. Monitoring Core Operations
To monitor connection initialization states, handshake speeds, or debug failures in real time, extract diagnostic data via journalctl:
#### View active operational state
```Bash
sudo systemctl status psiphon.service
```
#### View real-time streaming connections log 
```Bash
sudo journalctl -u psiphon.service -f -n 100
```

### 4. Routing Client Traffic
Route your downstream platform proxies or system-level network traffic profiles straight through the local daemon interface:

```Plaintext
SOCKS5 Proxy Host: 127.0.0.1
SOCKS5 Proxy Port: 22222
```

#### Example routing an application call via curl
```Bash
curl --socks5-hostname 127.0.0.1:22222 https://ifconfig.me
```
