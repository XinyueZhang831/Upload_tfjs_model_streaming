# Upload_tfjs_model_streaming

Dependency:

'''
npm install @tensorflow/tfjs

npm install @tensorflow-models/speech-commands
''



I trained my model with 25 "mute" and 25 "unmute" data on the demo page. 

"my-model.json" and "my-model.weights.bin" are the trained result.

"metadata.json" is created manually.

"testing2.html" is a simple sample code which includes javascript. I didn't seperate them into two files.

Current issue: I can load my model into the page but it doesn't react to my words. could be my html is disconnect with javascrip.

The following steps are how I made the model, upload the model to the server and test.

To use my testing2.html, 


## Step 1: pull the Speech command tfjs repo [https://github.com/tensorflow/tfjs-models/tree/master/speech-commands]


##  Step 2: train a new model with demo:
  
  In index.js, at line 660, change 
  
  '''
  await transferRecognizer.save();
  '''
  
  to
  
  '''
  await transferRecognizer.save('downloads://my-model');
  '''
  
  
  
  Give new words and click "Enter Tranfer words"
  
  Record data for each word more than 5 times
  
  Choose epoch and click "Start transfer learning"
  
  Click "Start" which is at the top of the demo page to test if this model works
  
  Click "Model IO" and "Save Model"
  
  "my-model.json" and "my-model.weights.bin" will be downloaded after clicking "Save Model"
  
  Note: we can use new words wav data to train model with soft-fft or browser-fft which is in "training" folder.
        https://github.com/tensorflow/tfjs-models/tree/master/speech-commands/training/soft-fft
        Soft-fft is easier to tain in terminal. 
        
## Step 3: Embed to website:

  "metadata.json" is created manually. "words" should contains the words except background noise and "frame" is 232. 
  
  "metadata.json" can be created automatically with browser-fft training code  which is located in "training"
  
  [https://github.com/tensorflow/tfjs-models/tree/master/speech-commands/training/browser-fft]
  
  Create a folder which contains "metadata.json", "my-model.json" and "my-model.weights.bin" into the same folder, to avoid CORS error, better to leave .html and .js file in to the same folder with the previous three files.

  In this folder, open terminal, use command "http-server" to uplod all files into local server. 
  
  ```
  xinyue@xinyuezhangs-MacBook-Pro ~ % http-server
  ```
  
  Check if uploaded by open http://localhost:8080/my-model.json and http://localhost:8080/metadata.json in browser.
    
  If use the original code from tfjs repo, in Javascipt modify the .create('BROWSER_FFT') line to 
  
  ```
  const customSpeechRecognizer = 
    speechCommands.create('BROWSER_FFT', null,   
                              'http://localhost:8080/my-model.json', 
                              'http://localhost:8080/metadata.json'); 
  ```
  
  This is the main step to load the custom model.
  
  Note: npm install http-server
  
  ```
  npm install --global http-server
  
  ```
  
## Step 4:

  Open "testing2.html" and test.
  
  If model is loaded correctly and yut catched the target word, the background of the word shoud turn to green
  
  
