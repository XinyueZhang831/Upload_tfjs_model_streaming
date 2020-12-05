# Upload tfjs model streaming

Dependency:

```
npm install @tensorflow/tfjs

npm install @tensorflow-models/speech-commands
```
In my testing2.html, I didn't use NPM package.

## Description:

I trained my model with 25 "mute" and 25 "unmute" data on the demo page. 

"my-model.json" and "my-model.weights.bin" are the trained result.

"metadata.json" is created manually.

"testing2.html" is a simple sample code which includes javascript. I didn't seperate them into two files.
#### To use my testing2.html, jump to step 3 and step 4.

## Current issue: 

I can load my model into the page but it doesn't react to my words. could be my html disconnects with javascrip.

The following steps are how I made the model, upload the model to the server and test.


## Step 1: pull the Speech command tfjs [repo](https://github.com/tensorflow/tfjs-models/tree/master/speech-commands)


##  Step 2: train a new model with demo:
  
  1. In index.js, at line 660, change 
  
  ```
  await transferRecognizer.save();
  ```
  to
  ```
  await transferRecognizer.save('downloads://my-model');
  ```
  
  
  2. Give new words and click "Enter Tranfer words".
  
  3. Record data for each word more than 5 times.
  
  4. Choose epoch and click "Start transfer learning".
  
  5. Click "Start" which is at the top of the demo page to test if this model works.
  
  6. Click "Model IO" and "Save Model".
  
  "my-model.json" and "my-model.weights.bin" will be downloaded after clicking "Save Model".
  
  Note: we can use new words wav data to train model with soft-fft or browser-fft which is in [training](https://github.com/tensorflow/tfjs-models/tree/master/speech-commands/training/soft-fft). Soft-fft will create a Sequencial model, and its weight file has different name from demo version.
        
## Step 3: Embed to website:

  1. Create "metadata.json" manually. "words" should contains the words except background noise and "frame" is 232. [Sample.](https://github.com/XinyueZhang831/Upload_tfjs_model_streaming/blob/main/audio%20model%20file/metadata.json)
  
   Note: "metadata.json" can be created automatically with browser-fft training code  which is located in [training](https://github.com/tensorflow/tfjs-models/tree/master/speech-commands/training/browser-fft)
  
  2. Create a folder which contains "metadata.json", "my-model.json" and "my-model.weights.bin", to avoid CORS error, better to leave .html and .js files in to the same folder with the previous three files.

  In this folder, open terminal, use command "http-server" to uplod all files into local server. 
  
  ```
  xinyue@xinyuezhangs-MacBook-Pro ~ % http-server
  ```
  
  3. Check if uploaded by open http://localhost:8080/my-model.json and http://localhost:8080/metadata.json in browser.
    
  
  Note: 
  
  1.To embed a custom model, in Javascipt modify the .create('BROWSER_FFT') line to 
  
  ```
  const customSpeechRecognizer = 
    speechCommands.create('BROWSER_FFT', null,   
                              'http://localhost:8080/my-model.json', 
                              'http://localhost:8080/metadata.json'); 
  ```
  
  This is the main step to load the custom model.
  
  2. npm install http-server
  
  ```
  npm install --global http-server
  
  ```
  
## Step 4:

  Open "testing2.html" and test.
  
  If model is loaded correctly and catched the target word, the background of the word should turn to green.
  
  
# Reference:

Import dependency: [Tensorflow Setup](https://www.tensorflow.org/js/tutorials/setup)

HTML Sample code: [Speech Recognition with TensorFlow.js](https://livecodestream.dev/post/speech-recognition-with-tensorflowjs/)

TFJS Speech Command: [Speech Command Recognizer](https://github.com/tensorflow/tfjs-models/tree/master/speech-commands)

Offcial TFJS Model save and load: [Save and load models](https://www.tensorflow.org/js/guide/save_load)

Simple tutorial of save model and upload to server: [Saving and Uploading a TensorFlow.js Speech model](https://handsondeeplearning.com/saving-and-uploading-a-tensorflow-js-speech-model/)
  
