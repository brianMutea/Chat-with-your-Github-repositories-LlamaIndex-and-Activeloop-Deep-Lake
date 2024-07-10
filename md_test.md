![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image1.png){width="2.7777777777777777in"
height="0.20833333333333334in"}![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image2.png){width="1.0in"
height="0.20833333333333334in"}![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image3.png){width="1.1666666666666667in"
height="0.20833333333333334in"}

+-----------------------------------+-----------------------------------+
| > **Campaign**                    | > AL-Tensorlake-P001-4            |
+===================================+===================================+
| > **Content No.**                 | > #2                              |
+-----------------------------------+-----------------------------------+
| > **Review Cycle**                | > #0                              |
+-----------------------------------+-----------------------------------+
| > **Sample Title #1**             | > Indexify: A deep-dive into      |
|                                   | > Andrew Huberman's Podcast       |
+-----------------------------------+-----------------------------------+
| > **Sample Title #2**             | > Indexify: How We Indexed Andrew |
|                                   | > Huberman\'s podcast and Why     |
|                                   | > this Matters                    |
+-----------------------------------+-----------------------------------+
| > **Sample Title #3**             | > Indexify: Unlocking             |
|                                   | > unstructured data using Andrew  |
|                                   | > Huberman's Podcast              |
+-----------------------------------+-----------------------------------+
| > **Sample Title #4**             | > Indexify: Extracting Lessons    |
|                                   | > from Steve Jobs using GenAI     |
+-----------------------------------+-----------------------------------+
| > **Reviewers**                   |                                   |
+-----------------------------------+-----------------------------------+
| > **Date Started**                | > 24 May 2024                     |
+-----------------------------------+-----------------------------------+
| > **Status**                      | > Under review                    |
+-----------------------------------+-----------------------------------+
| > **Notes**                       | > N/A                             |
+-----------------------------------+-----------------------------------+

> **Indexify: Extracting Lessons from**
>
> **Steve Jobs using GenAI**

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image4.png){width="6.22638779527559in"
height="3.548611111111111in"}

> Today, we have access to a vast amount of data, most of which exists
> in various formats and lacks a fixed arrangement (unstructured data),
> making interpreting challenging. One such form of data is video
> content. For instance, it is possible to extract text from video files
> and create a CSV file that includes the sentiments expressed in the
> text or use it as a knowledge base for an LLM-based application.
> However, doing this manually would be time-consuming and costly.
>
> Video data poses as the most challenging medium for extracting data.
> There are a few reasons why:
>
> ● There are many elements inside a video (speech, text, faces,
> objects, frames, etc.)\
> ● Difficult to extract the various elements of video data\
> ● Each element requires a different data extraction technique.
>
> Videos are not static - their context changes from one frame to the
> next.●\
> ● Because video data is unstructured, making sense of it is
> challenging. This makes indexing, searching, and understanding the
> content challenging.
>
> ● Extracting data at scale is expensive, and processing videos
> requires significant computational resources, especially when dealing
> with high-resolution videos.
>
> In this piece, we will guide you through extracting data from videos
> using Indexify. We will use as an example of transcript extraction.

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image5.png){width="3.3333333333333335in"
height="1.875in"}

> Steve Jobs is known as one of the world\'s most famous and successful\
> entrepreneurs. His unique approaches to business and marketing
> provoked the public\'s interest, which is why his speech on the
> importance of finding an exciting job you're passionate about drew
> attention and gained recognition. The speech\
> resonates with anyone who listens to it even today---find a job you
> truly love.
>
> In addition, rather than just extracting text from the video, Indexify
> can transform the data for semantic searches. With one seamless data
> pipeline, we will explore how **Indexify can chunk the extracted data,
> create its embeddings**,and **store indexes** from which we can
> retrieve key insights without watching the video.
>
> **Indexify** is a great tool for extracting and understanding data
> from videos. By the end of this article, you will know how to set up ,
> **Extraction Graphs**, **Extraction Policies**, and **search indexes**
> from the extracted data.
>
> **Why use Indexify to extract data from videos?**
>
> **Indexify** has a flexible workflow computing engine that
> accommodates various (among other forms of data like PDFs) and
> processes to transform it.
>
> Although there are other tools that overlap with its capabilities,
> Indexify outshines them in that:
>
> ● Indexify runs extraction and data processing workflows
> **asynchronously** and **reliably**.
>
> ● Indexify is **faster** since it doesn\'t rely on external schedulers
> like Kubernetes for task scheduling.
>
> ● It **tracks data lineage and updates extracted conten**t when the
> source changes, thus giving a clear view of where data comes from and
> how it has been processed.
>
> **How to extract data from videos with Indexify -**
>
> **Steve Jobs' commencement speech**
>
> Indexify has numerous pieces that create its workflow. Its system
> allows its users and developers to upload numerous **unstructured
> data** and wait for vector indexes to be updated while the extraction
> is running.
>
> Indexify has two major components:
>
> ● **Indexify server**: Has the ingestion system and the task scheduler
> for running extraction.
>
> ● : Compute functions that run models that transform or extract
> structured data or embed it from unstructured data.
>
> Indexify can run locally with all the components running on your
> computer. So, to follow along, you will need three terminals:
>
> ● The first terminal: Download and run the Indexify Server.
>
> ● The second terminal: Run Indexify extractors\
> ● The third terminal: Run our Python scripts to help load, extract,
> and store indexes in Indexify for querying.
>
> **Getting started**

In this section, we will set up Indexify to extract data from a video.
We will see how to

> add the required dependencies and extractors.
>
> **Starting the Indexify server**
>
> First, we need to kick off from a virtual environment.
>
> python -m venv ivenv
>
> By running the code above, we create a virtual environment called
> ivenv.
>
> **Activate the environment:**

+-----------------------------------------------------------------------+
| > source ivenv/bin/activate                                           |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Install the indexify-extractor-sdk and Indexify:**
>
> The extractor SDK allows new extraction capabilities and other readily
> available
>
> video extractors to be added.

+-----------------------------------------------------------------------+
| +------------------------------------------------------------------+  |
| | > pip3 install indexify-extractor-sdk==0.0.63 indexify==0.0.21   |  |
| +==================================================================+  |
| +------------------------------------------------------------------+  |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Download and run the Indexify server:**
>
> The Indexify server has the following functions:
>
> ● Evaluate the extraction policies and allocate tasks to extractors
> using a **coordinator**. This fast task scheduler creates thousands of
> tasks every second whenever data is ingested.
>
> ● Exposes HTTP and retrieval APIs via an **ingestion server**,
> allowing clients to upload content into Indexify, retrieve extracted
> data, or search over vector indexes.

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image6.png){width="5.656944444444444in"
height="2.5319444444444446in"}

> Run:

+-----------------------------------------------------------------------+
| > curl https://getindexify.ai \| sh                                   |
+=======================================================================+
+-----------------------------------------------------------------------+

> Once the server is downloaded, run the server. It is a long-running
> process:

+-----------------------------------------------------------------------+
| > ./indexify server -d                                                |
+=======================================================================+
+-----------------------------------------------------------------------+

> This is a long-running process. Open another terminal where we will
> install the required extractors and join them to the server.
>
> **Downloading and running extractors**
>
> Here, we will download and join the required extractors to the server.
> Extractors consume Content containing raw bytes of unstructured data
> and produce a list of Content and features from it.
>
> Indexify stores features like the embeddings into indexes for future
> retrieval.

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image7.png){width="4.156944444444444in"
height="2.125in"}

> We will need four extractors, which will work in the following order:
>
> 1\. **Video-audio extractor**: tensorlake/audio-extractor. Extracts
> audio from the video, which is easier to handle.
>
> 2\. **Audio extractor**: tensorlake/whisper-asr. Creates transcripts
> from the audio extracted.
>
> 3\. **Chunking extractor**: tensorlake/chunk-extractor.Chunks the
> extracted transcripts from the whisper-asr extractor.
>
> 4\. **Embedding extractor**: tensorlake/minilm-l6.Creates embeddings
> of the chunks that are kept in storage for semantic search.
>
> On the terminal, run the following one after the other to download
> each extractor:

+-----------------------------------------------------------------------+
| > indexify-extractor download hub://video/audio-extractor             |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > indexify-extractor download hub://audio/whisper-asr                 |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > indexify-extractor download hub://text/chunking                     |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > indexify-extractor download hub://embedding/minilm-l6               |
+=======================================================================+
+-----------------------------------------------------------------------+

> Once they are downloaded, run them on the server. This is a
> long-running process:

+-----------------------------------------------------------------------+
| +------------------------------------------------------------------+  |
| | > indexify-extractor join-server                                 |  |
| +==================================================================+  |
| +------------------------------------------------------------------+  |
+=======================================================================+
+-----------------------------------------------------------------------+

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image8.png){width="6.22638779527559in"
height="2.4458333333333333in"}

*You can access the server UI at . The UI lets you visualize the*

> *Content, Extractors, Indexes, and Extraction policies.*
>
> **Building the script for loading video and initiating extraction
> using**
>
> **Indexify:**
>
> Since we have the extractors downloaded and running, let's create a
> setup.py file
>
> that will:

+-----------------------------------+-----------------------------------+
| > ●\                              | > Download the video from         |
| > ●\                              | > YouTube.                        |
| > ●                               | >                                 |
|                                   | > Upload the video to Indexify.   |
|                                   | >                                 |
|                                   | > Initiate the extraction process |
|                                   | > with Indexify.                  |
+===================================+===================================+
+-----------------------------------+-----------------------------------+

> To download the video from YouTube, we need the pytube module. So you
> can
>
> install it by running:

+-----------------------------------------------------------------------+
| > pip install pytube                                                  |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Import necessary modules:**

+-----------------------------------------------------------------------+
| > import os                                                           |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > import yaml                                                         |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > from pytube import YouTube                                          |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > from indexify import IndexifyClient, ExtractionGraph                |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Intiaite Indexify client:**

+-----------------------------------------------------------------------+
| +------------------------------------------------------------------+  |
| | > client = IndexifyClient()                                      |  |
| +==================================================================+  |
| +------------------------------------------------------------------+  |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Create a function to download and load the YouTube video:**
>
> This function enables an optional error-handling block while
> downloading the video.
>
> defdownload_youtube_video(url, output_file):\
> \"\"\"\
> Download the YouTube video if it does not already exist in the current
> directory.
>
> Parameters:\
> url (str): The URL of the YouTube video.
>
> output_file (str): The name of the output file.
>
> \"\"\"\
> if os.path.exists(output_file):\
> print(f\"The file \'{output_file}\' already exists. Skipping
> download.\")\
> returnFalse
>
> try:\
> yt = YouTube(url)\
> video = (yt.streams\
> .filter(progressive=True, file_extension=\"mp4\")
> .order_by(\"resolution\")\
> .desc()\
> .first())\
> if video:\
> print(f\"Downloading \'{output_file}\'\...\")\
> video.download(filename=output_file)\
> print(\"Download complete!\")\
> returnTrue\
> else:\
> print(\"Video not found.\")\
> returnFalse\
> except Exception as e:\
> print(f\"An error occurred: {e}\")\
> returnFalse
>
> **Create a video_extraction_graph.yaml file:**
>
> The extraction graph is typically a data ingestion pipeline containing
> chained extraction policies that transform content through a series of
> extractors. We specify content_source to link the current extractor to
> the results of the previous extractor.
>
> So, in order when the data ingestion starts:
>
> ● The first extractor reads the video file and converts it into audio.
>
> ● The second extractor picks the extracted audio and \[\]'creates a
> transcription.
>
> It is a state-of-the-art automatic speech recognition that quickly
> converts spoken words from audio into text format.
>
> ● The third extractor gets the transcription, which is text, and
> splits it into smaller chunks.
>
> ● The last extractor creates embeddings of these text chunks, which
> Indexify stores as vector indexes to preserve their semantics, which
> is critical for semantic search.

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image9.png){width="6.22638779527559in"
height="2.4319444444444445in"}

Image by author: Extraction process

+-----------------------------------------------------------------------+
| > name: \"video2knowledgebase\"                                       |
+=======================================================================+
+-----------------------------------------------------------------------+

> extraction_policies:\
> - extractor: \"tensorlake/audio-extractor\"

+-----------------------------------------------------------------------+
| > name: \"audio_extractor\"                                           |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > \- extractor: \"tensorlake/whisper-asr\"                            |
+=======================================================================+
+-----------------------------------------------------------------------+

> name: \"transcriber\"

+-----------------------------------------------------------------------+
| > content_source: \"audio_extractor\"                                 |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > \- extractor: \"tensorlake/chunk-extractor\"                        |
+=======================================================================+
+-----------------------------------------------------------------------+

> name: \"transcripts_chunker\"\
> input_params:

+-----------------------------------------------------------------------+
| > chunk_size: 1000                                                    |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > overlap: 250                                                        |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > content_source: \"transcriber\"                                     |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > \- extractor: \"tensorlake/minilm-l6\"                              |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > name: \"transcription_embedding\"                                   |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > content_source: \"transcripts_chunker\"                             |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Create a function to upload and initiate extraction to Indexify:**
>
> The function will upload the video to Indexify and initiate the
> extraction through the extraction graph.
>
> We use some print statements and error handling to see what is
> happening. Note that the extraction may take a while, so you should
> monitor the terminal where the extractors are running for a successful
> response.
>
> In future, you can wait for the extraction to complete, which will
> give a great visualization of when the process is done.
>
> defupload_and_extract_video(output_file, extraction_graph_spec):
> \"\"\"

+-----------------------------------------------------------------------+
| > Upload the video file and initiate the extraction process.          |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > Parameters:                                                         |
+=======================================================================+
+-----------------------------------------------------------------------+

> output_file (str): The name of the output file.
>
> extraction_graph_spec: The extraction graph specification.
>
> \"\"\"\
> try:\
> *\# Create an extraction graph from the YAML file* extraction_graph =\
> ExtractionGraph.from_yaml(extraction_graph_spec)

+-----------------------------------------------------------------------+
| > client.create_extraction_graph(extraction_graph)                    |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > print(\"Uploading the video file\...\")                             |
+=======================================================================+
+-----------------------------------------------------------------------+

> content_id = client.upload_file(\"video2knowledgebase\", output_file)\
> print(f\"File uploaded successfully. Content ID:

+-----------------------------------------------------------------------+
| > {content_id}\")                                                     |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > print(\"Waiting for extraction to complete\...Might take            |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > time. Watch running extractors for a successful response!\")        |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > *\# client.wait_for_extraction(content_id)*                         |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > except Exception as e:                                              |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > print(f\"Error occurred during upload or extraction: {e}\")         |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Create the main block:**

+-----------------------------------------------------------------------+
| > if \_\_name\_\_ == \"\_\_main\_\_\":                                |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > url = \"https://www.youtube.com/watch?v=UF8uR6Z6KLc&t=6s\"          |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > output_file = \"stevejobs_commencement_vspeech.mp4\"                |
+=======================================================================+
+-----------------------------------------------------------------------+

  -----------------------------------------------------------------------
  download_successful = download_youtube_video(url, output_file)
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

+-----------------------------------------------------------------------+
| > *\# Read the .yaml file - extraction pipeline*                      |
+=======================================================================+
+-----------------------------------------------------------------------+

> with open(\"video_extraction_graph.yaml\", \"rb\") as file:

+-----------------------------------------------------------------------+
| > extraction_graph_spec = file.read()                                 |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > *\# upload the file and extract content to Indexify*                |
+=======================================================================+
+-----------------------------------------------------------------------+

> if download_successful or os.path.exists(output_file):
>
> upload_and_extract_video(output_file,

extraction_graph_spec)

> Finally, run setup.py:
>
> **Output:**

+-----------------------------------------------------------------------+
| > (ivenv) brianmutea@brianmutea:\~/BRIAN/Indexifystart\$ python       |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > setup.py                                                            |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > Downloading \'stevejobs_commencement_vspeech.mp4\'\...              |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > Download complete!                                                  |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > Uploading the video file\...                                        |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > File uploaded successfully. Content ID: 9ab2c9bc712c6171            |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > Waiting for extraction to complete\...Might take time. Watch        |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > running extractors for a successful response!                       |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Output** for Extraction Graph policies:

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image10.png){width="6.22638779527559in"
height="1.9416666666666667in"}

> **Output** for uploaded video file:

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image11.png){width="6.22638779527559in"
height="4.488888888888889in"}

> **Output** for Indexes created:

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image12.png){width="6.22638779527559in"
height="1.3055555555555556in"}

> **Output** content ingested:

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image13.png){width="6.22638779527559in"
height="3.501388888888889in"}

Viewing ingested content.

> Now that the data has been loaded and extracted and the indexes stored
> in Indexify, we can retrieve specific information.
>
> **How to retrieve specific information from the**
>
> **extracted video data using Indexify**
>
> Usually, to retrieve specific information from data using LLMs, we
> require custom and separate functions. For instance, while Indexify
> does that in one simple pipeline, you would require a function to load
> the data, extract audio, transcribe the audio, chunk and embed the
> chunks, and finally store the indexes. Indexify automatically
> abstracts all these functions from you.
>
> At this point, we can incorporate LLMs from OpenAI or open-source LMMs
> from Hugging Face to generate responses related to a query based on a
> video. By leveraging the simplicity offered by Indexify plus an LLM,
> we can create a chatbot to query key points.
>
> To make this possible, Indexify offers a **retrieval API** that allows
> us to retrieve the stored indexes that match a certain query. The
> retrieved indexes can then be passed to an LLM plus a prompt to create
> a responsive question-answering system. This will be covered in depth
> in a later tutorial.

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image14.png){width="6.22638779527559in"
height="2.2402777777777776in"}

Image by author: Indexify retrieval API

> Let's see how we can use Indexify\'s retrieval API\
> (client.search_index(query=index, query=\'question\', top_k=top_k)) to
> retrieve some indexes that match a specific query.
>
> Indexify stores the vector indexes from the embedding extractor so we
> can perform semantic searches on them. The search results contain
> chunks of text that match the query and their corresponding confidence
> scores.
>
> Create an app.py file and place the following code:

+-----------------------------------------------------------------------+
| > from indexify import IndexifyClient                                 |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > client = IndexifyClient()                                           |
+=======================================================================+
+-----------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| > *\# embedding index*                                                |
+=======================================================================+
+-----------------------------------------------------------------------+

> index = \"video2knowledgebase.transcription_embedding.embedding\"
> query = \"stay foolish\" *\# query*\
> results = client.search_index(name=index,\
> query=query,

  -----------------------------------------------------------------------
  top_k = 3)
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

+-----------------------------------------------------------------------+
| > print(\"\\n-\".join(\[item\[\"text\"\] for item in results\]))      |
+=======================================================================+
+-----------------------------------------------------------------------+

> **Output**(truncated):
>
> signed off. Stay hungry, stay foolish. And I have always wished
> thatfor myself. And now, as you graduate to begin anew, I wish that
> for you. Stay hungry. Stay foolish. Thank you all very much. Thank
> youall. Thank you all very much. Thank you. The proceeding program is
> copyrighted by Stanford University. Please visit us at Stanford.
>
> EADDU.
>
> -poetic touch\...
>
> Retrieval from the UI:

![](vertopal_1cca7ced9a474011a3312aff88d3b527/media/image15.png){width="6.22638779527559in"
height="3.5013877952755905in"}

Query indexes

> **Final thoughts and takeaways**
>
> Extracting video data can significantly enhance your capacity to
> interpret and leverage video data effectively. This data can be used
> with LLMs to create\
> LLM-based applications for question-answering tasks and video
> analysis. Although extracting video data is difficult, Indexify eases
> the process.
>
> This article shows how simple it is to extract valuable data from a
> video using Indexify. As you have learned, it is very easy and quick
> to set up and start using Indexify. Furthermore, we have explored
> Indexify\'s strengths to:

+-----------------------------------+-----------------------------------+
| > ●\                              | > Ingest video data\              |
| > ●\                              | > Convert the video into an audio |
| > ●\                              | > file.                           |
| > ●\                              | >                                 |
| > ●                               | > Read an audio file and          |
|                                   | > transcribe it into text.        |
|                                   | >                                 |
|                                   | > Split the transcribed text into |
|                                   | > smaller chunks.                 |
|                                   | >                                 |
|                                   | > Create embeddings of these      |
|                                   | > chunks and keep them in storage |
|                                   | > for semantic                    |
+===================================+===================================+
+-----------------------------------+-----------------------------------+

> search.
>
> Indexify does all that effortlessly in one very efficient data
> pipeline called an
>
> Extraction Graph.
>
> Indexify has a couple of extractors that developers can use for their
> specific use
>
> cases:
>
> ●
>
> ● (Read
>
> )
>
> ●
>
> ●
>
> ●
>
> **Further reading:**
>
> ● (linked when published)
>
> ●
>
> **Link up:**

+-----------------------------------+-----------------------------------+
| > ●\                              | > .\                              |
| > ●                               | > .                               |
+===================================+===================================+
+-----------------------------------+-----------------------------------+
