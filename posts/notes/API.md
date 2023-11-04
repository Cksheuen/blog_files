# 一. 每日一句励志英语

**接口地址**

> https://api.vvhan.com/api/en

**说明**

- 返回格式: JSON

- 请求方式: GET

- 请求示例(当天)

  https://api.vvhan.com/api/en

- 请求示例(随机)

  https://api.vvhan.com/api/en?type=sj

**参数说明**

| 名称 | 必填 | 类型   | 说明         |
| ---- | ---- | ------ | ------------ |
| type | 否   | string | 是否随机输出 |

**返回数据**

```json
{
    "success": true,
    "data" {
    	"month": "Oct",
    	"day":"18",
    	"zh":"井底之蛙，不知大海。",
    	"en":"The frog in the well knows nothing of the great ocean.",
    	"pic":"https:\/\/staticedu-wps.cache.iciba.com\/image\/0dfd35fcad9190747d8bd3cd2c91c758.jpg"
	}
}
```



# 二. 语音转文字

**接口地址:**

> https://api.vvhan.com/api/song

**说明**

- 返回格式: JSON

- 请求方式: GET

- 请求示例：https://api.vvhan.com/api/song?txt=内容

**参数说明**

| 名称 | 必填 | 类型   | 说明            |
| ---- | ---- | ------ | --------------- |
| txt  | 是   | string | 输入内容        |
| per  | 否   | string | 男声or女声(1-6) |


**返回数据**：

- 音频直接输出(音频输入地址)

# 三. 天气情况

**接口地址:**

> https://api.vvhan.com/api/weather

**说明：**

- 返回格式: JSON

- 请求方式: GET

- 请求示例(自动识别)：

  https://api.vvhan.com/api/weather

- 请求示例(输出当天)：

  https://api.vvhan.com/api/weather?ip=180.149.130.16

- 请求示例(输出当天)：

  https://api.vvhan.com/api/weather?city=徐州

- 请求示例(输出一周)：

  https://api.vvhan.com/api/weather?city=徐州&type=week

参数说明

| 名称 | 必填 | 类型   | 说明           |
| ---- | ---- | ------ | -------------- |
| city | 否   | string | 输入城市或县区 |
| ip   | 否   | string | 输入IP         |


**返回数据**

```json
{
  "data": {
    "yesterday": {
      "date": "30日星期三",
      "high": "高温 4℃",
      "fx": "西南风",
      "low": "低温 -1℃",
      "fl": "",
      "type": "雨夹雪"
    },
    "city": "西安",
    "forecast": [
      {
        "date": "31日星期四",
        "high": "高温 7℃",
        "fengli": "",
        "low": "低温 -6℃",
        "fengxiang": "西南风",
        "type": "小雪"
      },
      {
        "date": "1日星期五",
        "high": "高温 7℃",
        "fengli": "",
        "low": "低温 -4℃",
        "fengxiang": "东北风",
        "type": "多云"
      },
      {
        "date": "2日星期六",
        "high": "高温 7℃",
        "fengli": "",
        "low": "低温 -3℃",
        "fengxiang": "西南风",
        "type": "多云"
      },
      {
        "date": "3日星期天",
        "high": "高温 10℃",
        "fengli": "",
        "low": "低温 -1℃",
        "fengxiang": "南风",
        "type": "多云"
      },
      {
        "date": "4日星期一",
        "high": "高温 8℃",
        "fengli": "",
        "low": "低温 -3℃",
        "fengxiang": "东北风",
        "type": "多云"
      }
    ],
    "ganmao": "昼夜温差很大，易发生感冒，请注意适当增减衣服，加强自我防护避免感冒。",
    "wendu": "2"
  },
  "status": 1000,
  "desc": "OK"
}
```

# 四. 每日星座运势

**接口地址:**

> https://api.vvhan.com/api/horoscope

**说明:**

- 返回格式: JSON

- 请求方式：GET

- 请求示例（天蝎座今日运势）：

  https://api.vvhan.com/api/horoscope?type=scorpio&time=today

**参数说明:**

| 名称 | 必填 | 类型   | 说明                                                         |
| ---- | ---- | ------ | ------------------------------------------------------------ |
| type | 是   | string | 十二星座对应英文小写，aries, taurus, gemini, cancer, leo, virgo, libra, scorpio, sagittarius, capricorn, aquarius, pisces |
| time | 是   | string | 今日明日一周等运势,today, nextday, week, month, year, love   |


**返回数据:**

```json
{
  "success": true,
  "data": {
    "title": "天蝎座今日运势",
    "time": "8月9日",
    "fortune": {
      "all": 4,
      "love": 4,
      "work": 4,
      "money": 4
    },
    "index": {
      "health": "91%",
      "discuss": "94%"
    },
    "luckycolor": "白色",
    "luckynumber": "4",
    "luckyconstellation": "白羊座",
    "shortcomment": "静待时机的来临",
    "fortunetext": {
      "all": "运势不错，也会有心想事成的惊喜。你能嗅到机会在附近，所以总是保持蓄势待发的姿态，有足够耐心静待时机的来临。生活方面建议保持适度的健身运动，能够帮助保持最佳的状态，也可以合理分配休闲娱乐的时间。",
      "love": "单身的抓住近水楼台的优势，脱单在望。恋爱中的制定两人计划，保持很高的默契。",
      "work": "能将自己的优势发挥得恰到好处，懂得收放自如，而且对于机会的把握非常到位。",
      "money": "偏财运正当旺，会为你带来很多的商机，而且敢于大胆出手，更容易得到进账机会。",
      "health": "神采飞扬的一天，但也要注意做好健康的把关，尤其是改掉躺着玩手机的坏习惯。"
    }
  }
}
```

# 五. 网易云每日歌曲

**接口地址:**

> https://api.vvhan.com/api/rand.music

**说明:**

- 返回格式: JSON

- 请求方式：GET

- 请求示例(JSON输出)(热歌榜/新歌榜/飙升榜/原创)

  https://api.vvhan.com/api/rand.music?type=json&sort=热歌榜

  https://api.vvhan.com/api/rand.music?type=json&sort=新歌榜

  https://api.vvhan.com/api/rand.music?type=json&sort=飙升榜

  https://api.vvhan.com/api/rand.music?type=json&sort=原创

- 请求示例(MP3输出)(热歌榜/新歌榜/飙升榜/原创)

  https://api.vvhan.com/api/rand.music?sort=热歌榜

  https://api.vvhan.com/api/rand.music?sort=新歌榜

  https://api.vvhan.com/api/rand.music?sort=飙升榜

  https://api.vvhan.com/api/rand.music?sort=原创

- 请求示例(全部输出)：

  https://api.vvhan.com/api/rand.music?type=all&sort=热歌榜

**参数说明:**

| 名称 | 必填 | 类型   | 说明         |
| ---- | ---- | ------ | ------------ |
| sort | 是   | string | 输入歌单名称 |
| type | 否   | string | 输出类型     |


**返回数据:**

```json
{
  "sort": "热歌榜",
  "id": 1359356908,
  "name": "晚安",
  "auther": "颜人中",
  "mp3url": "https:\/\/music.163.com\/song\/media\/outer\/url?id=1359356908.mp3",
  "imgurl": "https:\/\/p1.music.126.net\/8N1fsMRm2L5HyZccc6I3ew==\/109951164007377169.jpg"
}
```

