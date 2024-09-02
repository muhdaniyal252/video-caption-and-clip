# Pre Requisites
To run the project, 

1. you will need to have follwoing things installed on you system

    * Python -  ```apt-get install python3 python3-dev python-pip```
    * Image Magic ```apt-get install imagemagick libmagick++-dev -y ```
        * run this command in with __root__ user ```cat /etc/ImageMagick-6/policy.xml  |sed 's/none/read,write/g'> /etc/ImageMagick-6/policy.xml```
2. Gemini API
* You will need to have a Gemini API account to run the project. You can have you API from the [Generate API](https://aistudio.google.com/app/apikey) page.
    * We are using __Gemini 1.5 Flash__, you can have which ever version you like. You me see the available models from it's [Pricing Page](https://ai.google.dev/pricing)


# Project Setup

To run the project, ```cd``` into the directory.

Run command ```pip install -r requirements.txt```


# Functionality

The project basically does 2 things
1. ```caption_video```  takes __video_path__ as an input and save the new video in same directory as input video but with captions inside it.
    * The ides is to extract the one frame from each second of video (randomly selected frame from every second) and get those frames captionated using the [Salesforce/blip-image-captioning-large](https://huggingface.co/Salesforce/blip-image-captioning-large) model. After getting the caption for each second in the video, we seek LLM's help to make our captions more sensible get the time frames from it. For that, we wrote a suitable prompt. then we converted the LLM response to json (we asked to return json format so that we can create json object from json string). and then using the moviepy package from python, we put captoins over the video and save into the same directory as input video.
2. ```clip_video``` takes __video_path and scene__ as input and save the new video in same directory as input video but only the segment described in the scene.
    * The ides is to extract the one frame from each second of video (randomly selected frame from every second) and get those frames captionated using the [Salesforce/blip-image-captioning-large](https://huggingface.co/Salesforce/blip-image-captioning-large) model. After getting the caption for each second in the video, we seek LLM's help to extract the segment of video that best describes the given scene. then we converted the LLM response to json (we asked to return json format so that we can create json object from json string). and then using the moviepy package from python, we clipped the video and save that segment into same directory as input video


# Limitations
* The accuracy is good. But instead of 1 frame from each second, if there is a model that could take the whole video (or chunk of a video), that would produce better result, like [Vid2Seq](https://arxiv.org/abs/2302.14115) as it would have whole context in front of it instead of just few frames.  
* No extensive exctption handling
* No extensive testing

