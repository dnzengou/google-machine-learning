<h1> Using Machine Learning APIs </h1>

First, visit <a href="http://console.cloud.google.com/apis">API console</a>, choose "Credentials" on the left-hand menu.  Choose "Create Credentials" and generate an API key for your application. You should probably restrict it by IP address to prevent abuse, but for now, just  leave that field blank and delete the API key after trying out this demo.

Copy-paste your API Key here:


```python
!sudo chown -R jupyter:jupyter /home/jupyter/training-data-analyst
```


```python
APIKEY="AIzaSyB4XGsnQuBa0f2Z7HatlrPXupm0TkdQ-jI"  # Replace with your API key
```

<b> Note: Make sure you generate an API Key and replace the value above. The sample key will not work.</b>


<h2> Invoke Translate API </h2>


```python
# running Translate API
from googleapiclient.discovery import build
service = build('translate', 'v2', developerKey=APIKEY)

# use the service
inputs = ['is it really this easy?', 'amazing technology', 'wow']
outputs = service.translations().list(source='en', target='fr', q=inputs).execute()
# print outputs
for input, output in zip(inputs, outputs['translations']):
  print("{0} -> {1}".format(input, output['translatedText']))
```

    is it really this easy? -> est-ce vraiment si simple?
    amazing technology -> technologie incroyable
    wow -> sensationnel


<h2> Invoke Vision API </h2>

The Vision API can work off an image in Cloud Storage or embedded directly into a POST message. I'll use Cloud Storage and do OCR on this image: <img src="https://storage.googleapis.com/cloud-training-demos/vision/sign2.jpg" width="200" />.  That photograph is from http://www.publicdomainpictures.net/view-image.php?image=15842



```python
# Running Vision API
import base64
IMAGE="gs://cloud-training-demos/vision/sign2.jpg"
vservice = build('vision', 'v1', developerKey=APIKEY)
request = vservice.images().annotate(body={
        'requests': [{
                'image': {
                    'source': {
                        'gcs_image_uri': IMAGE
                    }
                },
                'features': [{
                    'type': 'TEXT_DETECTION',
                    'maxResults': 3,
                }]
            }],
        })
responses = request.execute(num_retries=3)
print(responses)
```

    {'responses': [{'textAnnotations': [{'locale': 'zh', 'description': '请您爱护和保\n护卫生创建优\n美水环境\n', 'boundingPoly': {'vertices': [{'x': 121, 'y': 80}, {'x': 1071, 'y': 80}, {'x': 1071, 'y': 655}, {'x': 121, 'y': 655}]}}, {'description': '请', 'boundingPoly': {'vertices': [{'x': 160, 'y': 80}, {'x': 359, 'y': 80}, {'x': 359, 'y': 239}, {'x': 160, 'y': 239}]}}, {'description': '您', 'boundingPoly': {'vertices': [{'x': 361, 'y': 80}, {'x': 487, 'y': 80}, {'x': 487, 'y': 239}, {'x': 361, 'y': 239}]}}, {'description': '爱护', 'boundingPoly': {'vertices': [{'x': 480, 'y': 112}, {'x': 767, 'y': 112}, {'x': 767, 'y': 239}, {'x': 480, 'y': 239}]}}, {'description': '和', 'boundingPoly': {'vertices': [{'x': 784, 'y': 112}, {'x': 919, 'y': 112}, {'x': 919, 'y': 239}, {'x': 784, 'y': 239}]}}, {'description': '保', 'boundingPoly': {'vertices': [{'x': 936, 'y': 104}, {'x': 1071, 'y': 104}, {'x': 1071, 'y': 231}, {'x': 936, 'y': 231}]}}, {'description': '护', 'boundingPoly': {'vertices': [{'x': 121, 'y': 280}, {'x': 327, 'y': 280}, {'x': 327, 'y': 451}, {'x': 121, 'y': 451}]}}, {'description': '卫生', 'boundingPoly': {'vertices': [{'x': 329, 'y': 280}, {'x': 671, 'y': 280}, {'x': 671, 'y': 451}, {'x': 329, 'y': 451}]}}, {'description': '创建', 'boundingPoly': {'vertices': [{'x': 673, 'y': 280}, {'x': 987, 'y': 280}, {'x': 987, 'y': 451}, {'x': 673, 'y': 451}]}}, {'description': '优', 'boundingPoly': {'vertices': [{'x': 989, 'y': 280}, {'x': 1043, 'y': 280}, {'x': 1043, 'y': 451}, {'x': 989, 'y': 451}]}}, {'description': '美', 'boundingPoly': {'vertices': [{'x': 152, 'y': 504}, {'x': 295, 'y': 504}, {'x': 295, 'y': 647}, {'x': 152, 'y': 647}]}}, {'description': '水', 'boundingPoly': {'vertices': [{'x': 312, 'y': 504}, {'x': 455, 'y': 504}, {'x': 455, 'y': 647}, {'x': 312, 'y': 647}]}}, {'description': '环境', 'boundingPoly': {'vertices': [{'x': 464, 'y': 504}, {'x': 767, 'y': 504}, {'x': 767, 'y': 655}, {'x': 464, 'y': 655}]}}], 'fullTextAnnotation': {'pages': [{'property': {'detectedLanguages': [{'languageCode': 'zh', 'confidence': 0.75}]}, 'width': 1280, 'height': 818, 'blocks': [{'property': {'detectedLanguages': [{'languageCode': 'zh', 'confidence': 0.75}]}, 'boundingBox': {'vertices': [{'x': 121, 'y': 80}, {'x': 1071, 'y': 80}, {'x': 1071, 'y': 655}, {'x': 121, 'y': 655}]}, 'paragraphs': [{'property': {'detectedLanguages': [{'languageCode': 'zh', 'confidence': 0.75}]}, 'boundingBox': {'vertices': [{'x': 121, 'y': 80}, {'x': 1071, 'y': 80}, {'x': 1071, 'y': 655}, {'x': 121, 'y': 655}]}, 'words': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 160, 'y': 80}, {'x': 359, 'y': 80}, {'x': 359, 'y': 239}, {'x': 160, 'y': 239}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 160, 'y': 80}, {'x': 359, 'y': 80}, {'x': 359, 'y': 239}, {'x': 160, 'y': 239}]}, 'text': '请'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 361, 'y': 80}, {'x': 487, 'y': 80}, {'x': 487, 'y': 239}, {'x': 361, 'y': 239}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 361, 'y': 80}, {'x': 487, 'y': 80}, {'x': 487, 'y': 239}, {'x': 361, 'y': 239}]}, 'text': '您'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 480, 'y': 112}, {'x': 767, 'y': 112}, {'x': 767, 'y': 239}, {'x': 480, 'y': 239}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 480, 'y': 112}, {'x': 615, 'y': 112}, {'x': 615, 'y': 239}, {'x': 480, 'y': 239}]}, 'text': '爱'}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 632, 'y': 112}, {'x': 767, 'y': 112}, {'x': 767, 'y': 239}, {'x': 632, 'y': 239}]}, 'text': '护'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 784, 'y': 112}, {'x': 919, 'y': 112}, {'x': 919, 'y': 239}, {'x': 784, 'y': 239}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 784, 'y': 112}, {'x': 919, 'y': 112}, {'x': 919, 'y': 239}, {'x': 784, 'y': 239}]}, 'text': '和'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 936, 'y': 104}, {'x': 1071, 'y': 104}, {'x': 1071, 'y': 231}, {'x': 936, 'y': 231}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}], 'detectedBreak': {'type': 'EOL_SURE_SPACE'}}, 'boundingBox': {'vertices': [{'x': 936, 'y': 104}, {'x': 1071, 'y': 104}, {'x': 1071, 'y': 231}, {'x': 936, 'y': 231}]}, 'text': '保'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 121, 'y': 280}, {'x': 327, 'y': 280}, {'x': 327, 'y': 451}, {'x': 121, 'y': 451}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 121, 'y': 280}, {'x': 327, 'y': 280}, {'x': 327, 'y': 451}, {'x': 121, 'y': 451}]}, 'text': '护'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 329, 'y': 280}, {'x': 671, 'y': 280}, {'x': 671, 'y': 451}, {'x': 329, 'y': 451}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 329, 'y': 280}, {'x': 499, 'y': 280}, {'x': 499, 'y': 451}, {'x': 329, 'y': 451}]}, 'text': '卫'}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 500, 'y': 280}, {'x': 671, 'y': 280}, {'x': 671, 'y': 451}, {'x': 500, 'y': 451}]}, 'text': '生'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 673, 'y': 280}, {'x': 987, 'y': 280}, {'x': 987, 'y': 451}, {'x': 673, 'y': 451}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 673, 'y': 280}, {'x': 815, 'y': 280}, {'x': 815, 'y': 451}, {'x': 673, 'y': 451}]}, 'text': '创'}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 816, 'y': 280}, {'x': 987, 'y': 280}, {'x': 987, 'y': 451}, {'x': 816, 'y': 451}]}, 'text': '建'}]}, {'property': {'detectedLanguages': [{'languageCode': 'zh'}]}, 'boundingBox': {'vertices': [{'x': 989, 'y': 280}, {'x': 1043, 'y': 280}, {'x': 1043, 'y': 451}, {'x': 989, 'y': 451}]}, 'symbols': [{'property': {'detectedLanguages': [{'languageCode': 'zh'}], 'detectedBreak': {'type': 'EOL_SURE_SPACE'}}, 'boundingBox': {'vertices': [{'x': 989, 'y': 280}, {'x': 1043, 'y': 280}, {'x': 1043, 'y': 451}, {'x': 989, 'y': 451}]}, 'text': '优'}]}, {'boundingBox': {'vertices': [{'x': 152, 'y': 504}, {'x': 295, 'y': 504}, {'x': 295, 'y': 647}, {'x': 152, 'y': 647}]}, 'symbols': [{'boundingBox': {'vertices': [{'x': 152, 'y': 504}, {'x': 295, 'y': 504}, {'x': 295, 'y': 647}, {'x': 152, 'y': 647}]}, 'text': '美'}]}, {'boundingBox': {'vertices': [{'x': 312, 'y': 504}, {'x': 455, 'y': 504}, {'x': 455, 'y': 647}, {'x': 312, 'y': 647}]}, 'symbols': [{'boundingBox': {'vertices': [{'x': 312, 'y': 504}, {'x': 455, 'y': 504}, {'x': 455, 'y': 647}, {'x': 312, 'y': 647}]}, 'text': '水'}]}, {'boundingBox': {'vertices': [{'x': 464, 'y': 504}, {'x': 767, 'y': 504}, {'x': 767, 'y': 655}, {'x': 464, 'y': 655}]}, 'symbols': [{'boundingBox': {'vertices': [{'x': 464, 'y': 512}, {'x': 615, 'y': 512}, {'x': 615, 'y': 647}, {'x': 464, 'y': 647}]}, 'text': '环'}, {'property': {'detectedBreak': {'type': 'LINE_BREAK'}}, 'boundingBox': {'vertices': [{'x': 624, 'y': 504}, {'x': 767, 'y': 504}, {'x': 767, 'y': 655}, {'x': 624, 'y': 655}]}, 'text': '境'}]}]}], 'blockType': 'TEXT'}]}], 'text': '请您爱护和保\n护卫生创建优\n美水环境\n'}}]}



```python
foreigntext = responses['responses'][0]['textAnnotations'][0]['description']
foreignlang = responses['responses'][0]['textAnnotations'][0]['locale']
print(foreignlang, foreigntext)
```

    zh 请您爱护和保
    护卫生创建优
    美水环境
    


<h2> Translate sign </h2>


```python
inputs=[foreigntext]
outputs = service.translations().list(source=foreignlang, target='en', q=inputs).execute()
# print(outputs)
for input, output in zip(inputs, outputs['translations']):
  print("{0} -> {1}".format(input, output['translatedText']))
```

    请您爱护和保
    护卫生创建优
    美水环境
     -> Please care and protect sanitation to create a beautiful water environment


<h2> Sentiment analysis with Language API </h2>

Let's evaluate the sentiment of some famous quotes using Google Cloud Natural Language API.


```python
lservice = build('language', 'v1beta1', developerKey=APIKEY)
quotes = [
  'To succeed, you must have tremendous perseverance, tremendous will.',
  'It’s not that I’m so smart, it’s just that I stay with problems longer.',
  'Love is quivering happiness.',
  'Love is of all passions the strongest, for it attacks simultaneously the head, the heart, and the senses.',
  'What difference does it make to the dead, the orphans and the homeless, whether the mad destruction is wrought under the name of totalitarianism or in the holy name of liberty or democracy?',
  'When someone you love dies, and you’re not expecting it, you don’t lose her all at once; you lose her in pieces over a long time — the way the mail stops coming, and her scent fades from the pillows and even from the clothes in her closet and drawers. '
]
for quote in quotes:
  response = lservice.documents().analyzeSentiment(
    body={
      'document': {
         'type': 'PLAIN_TEXT',
         'content': quote
      }
    }).execute()
  polarity = response['documentSentiment']['polarity']
  magnitude = response['documentSentiment']['magnitude']
  print('POLARITY=%s MAGNITUDE=%s for %s' % (polarity, magnitude, quote))
```

    POLARITY=1 MAGNITUDE=0.8 for To succeed, you must have tremendous perseverance, tremendous will.
    POLARITY=-1 MAGNITUDE=0.2 for It’s not that I’m so smart, it’s just that I stay with problems longer.
    POLARITY=1 MAGNITUDE=0.9 for Love is quivering happiness.
    POLARITY=1 MAGNITUDE=0.9 for Love is of all passions the strongest, for it attacks simultaneously the head, the heart, and the senses.
    POLARITY=-1 MAGNITUDE=0.2 for What difference does it make to the dead, the orphans and the homeless, whether the mad destruction is wrought under the name of totalitarianism or in the holy name of liberty or democracy?
    POLARITY=-1 MAGNITUDE=0.3 for When someone you love dies, and you’re not expecting it, you don’t lose her all at once; you lose her in pieces over a long time — the way the mail stops coming, and her scent fades from the pillows and even from the clothes in her closet and drawers. 


<h2> Speech API </h2>

The Speech API can work on streaming data, audio content encoded and embedded directly into the POST message, or on a file on Cloud Storage. Here I'll pass in this <a href="https://storage.googleapis.com/cloud-training-demos/vision/audio.raw">audio file</a> in Cloud Storage.


```python
sservice = build('speech', 'v1', developerKey=APIKEY)
response = sservice.speech().recognize(
    body={
        'config': {
            'languageCode' : 'en-US',
            'encoding': 'LINEAR16',
            'sampleRateHertz': 16000
        },
        'audio': {
            'uri': 'gs://cloud-training-demos/vision/audio.raw'
            }
        }).execute()
print(response)
```

    {'results': [{'alternatives': [{'transcript': 'how old is the Brooklyn Bridge', 'confidence': 0.98314303}]}]}



```python
print(response['results'][0]['alternatives'][0]['transcript'])
print('Confidence=%f' % response['results'][0]['alternatives'][0]['confidence'])
```

    how old is the Brooklyn Bridge
    Confidence=0.983143


<h2> Clean up </h2>

Remember to delete the API key by visiting <a href="http://console.cloud.google.com/apis">API console</a>.

If necessary, commit all your notebooks to git.

If you are running Datalab on a Compute Engine VM or delegating to one, remember to stop or shut it down so that you are not charged.


## Challenge Exercise

Here are a few portraits from the Metropolitan Museum of Art, New York (they are part of a [BigQuery public dataset](https://bigquery.cloud.google.com/dataset/bigquery-public-data:the_met) ):

* gs://cloud-training-demos/images/met/APS6880.jpg
* gs://cloud-training-demos/images/met/DP205018.jpg
* gs://cloud-training-demos/images/met/DP290402.jpg
* gs://cloud-training-demos/images/met/DP700302.jpg

Use the Vision API to identify which of these images depict happy people and which ones depict unhappy people.

Hint (highlight to see): <p style="color:white">You will need to look for joyLikelihood and/or sorrowLikelihood from the response.</p>

Copyright 2020 Google Inc.
Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.


```python

```


```python

```
