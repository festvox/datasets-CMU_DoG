# ConvAI_data

## Overview
This paper introduces a document grounded dataset for conversations. We define "Document Grounded Conversations" as conversations that are about the contents of a specified document. In this dataset the specified documents were Wikipedia articles about popular movies. The dataset contains 4112 conversations with an average of 21.43 turns per conversation. This positions this dataset to not only provide a relevant chat history while generating responses but also provide a source of information that the models could use. We describe two neural architectures that provide benchmark performance on the task of generating the next response. We also evaluate our models for engagement and fluency, and find that the information from the document helps in generating more engaging and fluent responses.

## Statistics

* Total number of conversations(having at least 1 turns): 4112
* Rating
  * The whole data is splited into 3 parts, from rate 1(lower) to 3(better).
  * Rate 1: Low BLEU score(< 0.1), or the number of turns lower than 10
  * Rate 2: All the conversations that do not fit in rate 1 or 3
  * Rate 3: Conversations with more than 12 turns, with BLEU score over one std
* Overall stats based on ratings

  Stats\Ratings                       | Rate 1         | Rate 2        | Rate 3
  ------------                        | ----           | ----          | ---
  Total Number of conversations       | 1443           | 2142          | 527
  Total Number of utterances          | 28536          | 80104         | 21360
  Mean/Std utterances per conversations    | 19.77 ± 13.68  | 35.39 ± 8.48  | 40.53 ± 12.92
  Mean/Std length of utterances       | 7.51 ± 50.19   | 10.56 ± 8.51  | 16.57 ± 15.23

## Data Format
* Conversations Data
  * All the conversation data files are in the 'Conversations' folder, with train/validation/test split.
  * Each conversation file is a json file. Fields in the file includes
    * 'date': the time the file is created.
    * 'docLinkIdx': the index of the document. See the 'index' part for next section for details.
    * 'docType': (Do we need to include this?).
    * 'history': a list of json object containing the whole chat history. Fields in the json objects in the list includes
      * 'text': the text of each utterance.
      * 'utcTimestamp': the server utc timestamp of this utterance.
      * 'uid': the user id of this utterance.
      * 'docIdx': the current document index of the file. Could be missing some files. (explain better)
    * 'rating': A number from 1 or 2 or 3. A larger number means the quality of the conversation is better. (where Rating 3 means good quality conversations and Rating 1 means bad quality conversation. To understand how this number is calculated please refer to our paper.)
    * 'uid1LogInTime', 'uid1LogOutTime', 'uid2LogInTime', 'uid2LogOutTime': The log in and log out time of user1 and user2. Any of the field could be missing. (why)
    * 'uid1response', 'uid2response': a json object contains the status and response of user after finishing the conversation. Fields in the object includes
      * 'type': should be one of ['finish', 'abadon','abandonWithouAnsweringFeedbackQuestion']. 'finish' means the user successfully finishes the conversation, either by completing 12 or 15 turns or in the way that the other user leaves the conversation first. 'abandon' means the user abandons the conversation in the middle, but entering the feedback page. 'abandonWithouAnsweringFeedbackQuestion' means the user just disconnects or closes the web page without providing the feedback.
      * 'response': the answer to the post-conversation questions. The meaning of the option presents in the data is as follows (rephrase):
        * For type 'finish'
          * 1: The conversation is understandable.
          * 2: The other user is actively responding me.
          * 3: The conversation goes smoothly.
        * For type 'abadon'
          * 1: The other user is too rude.
          * 2: I don't know how to proceed with the conversation.
          * 3: The other user is not responding to me.
        * For chatType 0 (both users were given the document)
          * 4: I have watched the movie before.
          * 5: I have not watched the movie before.
        * For chatType 1 (only one user was given the document)
          * 4: I will watch the movie after the other user's introduction.
          * 5: I will not watch the movie after the other user's introduction.
      * 'feedback': the user-entered feedback after the conversation.
    * 'user1_id', 'user2_id': the generated user id of user1 and user2.
    * 'status': 0 means the conversation finished with error(At least one user abandoned the conversations). 1 means the conversation finished correctly.
    * 'whoSawDoc': Should be one of ['user1'], ['user2'], ['user1', 'user2']. Indicating which user read the document.

* Wiki Document Data
  * All the wiki document data files are in the 'WikiData' folder
  * An example of the data is included in the repository.
  * Each file is a json file. Fields in the file includes
    * '0', '1', '2', '3': the introductions and key scenes in the movie. The number corresponds to the 'docIdx' field in the 'history' in the conversation data file. '0' corresponds to the introductions, and '1', '2', and '3' are the index of key scenes.
    * The meaning of each field in '0':
      * 'cast': the cast of the movie, possibly with a brief introduction of the role.
      * 'critical_response': a list of selected critical responses of the movie. Some of them are positive reviews, while some are negatives.
      * 'director': the director of the movie.
      * 'genre': the genre of the movie.
      * 'introduction': the introduction of the movie, with some basic information.
      * 'movieName': the name of the movie.
      * 'rating': a list of ratings from major review websites for this movie. The ratings usually includes some of the Rotten Tomatoes, IMDB, CinemaScore, or Metacritic.
      * 'year': the year the movie came out.
     * 'index': the index of the movie, corresponding to the 'docLinkIdx' field in conversation data. (more explanantion)

## References

This repo contains the code and data of the following paper:
>A Dataset for Document Grounded Conversations. *Kangyan Zhou, Shrimai Prabhumoye, Alan W Black*. EMNLP 2018. [arXiv](https://arxiv.org/pdf/xxx.pdf)

    @inproceedings{document_grounded_emnlp18,
    title={A Dataset for Document Grounded Conversations},
    author={Zhou, Kangyan and Prabhumoye, Shrimai and Black, Alan W},
    year={2018},
    booktitle={Proc. EMNLP}
    }
