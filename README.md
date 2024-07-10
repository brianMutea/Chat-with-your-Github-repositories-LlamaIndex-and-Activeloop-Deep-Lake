# Chat with your Github repositories -> LlamaIndex and Activeloop DeepLake

[See Notebook](https://github.com/brianMutea/Chat-with-your-Github-repositories-LlamaIndex-and-Activeloop-Deep-Lake.ipynb/blob/main/Chat_with_your_Github_repositories_LlamaIndex_and_Activeloop_Deep_Lake.ipynb)

Chatting with this repo:
[LlamaIndex-Precision-and-Simplicity-in-Information-Retrieval](https://github.com/brianMutea/LlamaIndex-Precision-and-Simplicity-in-Information-Retrieval)

```Python
def parse_github_url(url):
  pattern = r"https://github\.com/([^/]+)/([^/]+)"
  match = re.match(pattern, url)
  return match.groups() if match else (None, None)


def validate_owner_repo(owner, repo):
  return bool(owner) and bool(repo)


def initialize_github_client():
  github_token = os.getenv("GITHUB_TOKEN")
  return GithubClient(github_token)


def main():
  # Check for OpenAI API key
  openai_api_key = os.getenv("OPENAI_API_KEY")
  if not openai_api_key:
      raise EnvironmentError("OpenAI API key not found in environment variables")

  # Check for GitHub Token
  github_token = os.getenv("GITHUB_TOKEN")
  if not github_token:
      raise EnvironmentError("GitHub token not found in environment variables")

  # Check for Activeloop Token
  active_loop_token = os.getenv("ACTIVELOOP_TOKEN")
  if not active_loop_token:
      raise EnvironmentError("Activeloop token not found in environment variables")

  github_client = initialize_github_client()
  download_loader("GithubRepositoryReader")

  github_url = input("Please enter the GitHub repository URL: ")
  owner, repo = parse_github_url(github_url)

  while True:
      owner, repo = parse_github_url(github_url)
      if validate_owner_repo(owner, repo):
          loader = GithubRepositoryReader(
              github_client,
              owner=owner,
              repo=repo,
              filter_file_extensions=(
                  [".ipynb"],
                  GithubRepositoryReader.FilterType.INCLUDE,
              ),
              verbose=False,
              concurrent_requests=5,
          )
          print(f"Loading {repo} repository by {owner}")
          docs = loader.load_data(branch="main")
          print("Documents uploaded:")
          for doc in docs:
              print(doc.metadata)
          break  # Exit the loop once the valid URL is processed
      else:
          print("Invalid GitHub URL. Please try again.")
          github_url = input("Please enter the GitHub repository URL: ")

  print("Uploading to vector store...")

  # ====== Create vector store and upload data ======

  vector_store = DeepLakeVectorStore(
      dataset_path= dataset_path,
      overwrite=True,
      runtime={"tensor_db": True},
  )

  storage_context = StorageContext.from_defaults(vector_store = vector_store)
  index = VectorStoreIndex.from_documents(
      docs, storage_context = storage_context
      )
  query_engine = index.as_query_engine()

  # Include a simple question to test.
  intro_question = "What is the repository about?"
  print(f"Test question: {intro_question}")
  print("=" * 50)
  answer = query_engine.query(intro_question)

  print(f"Answer: {textwrap.fill(str(answer), 100)} \n")
  while True:
      user_question = input("Please enter your question (or type 'exit' to quit): ")
      if user_question.lower() == "exit":
          print("Exiting, thanks for chatting!")
          break

      print(f"Your question: {user_question}")
      print("=" * 50)

      answer = query_engine.query(user_question)
      print(f"Answer: {textwrap.fill(str(answer), 100)} \n")

if __name__ == "__main__":
  main()
```

### Results

![chatting with repo](https://github.com/brianMutea/Chat-with-your-Github-repositories-LlamaIndex-and-Activeloop-Deep-Lake.ipynb/blob/main/Screenshot%20from%202024-02-07%2012-15-29.png)

=====================================================================================================

[See Notebook](https://github.com/brianMutea/Chat-with-your-Github-repositories-LlamaIndex-and-Activeloop-Deep-Lake.ipynb/blob/main/Chat_with_your_Github_repositories_LlamaIndex_and_Activeloop_Deep_Lake.ipynb)

Chatting with this repo:
[LlamaIndex-Precision-and-Simplicity-in-Information-Retrieval](https://github.com/brianMutea/LlamaIndex-Precision-and-Simplicity-in-Information-Retrieval)











============================================================


<!-----



Conversion time: 0.344 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β36
* Tue Jul 09 2024 22:44:14 GMT-0700 (PDT)
* Source doc: Tell me about yourself:
----->


**Tell me about yourself: \
** \
I began my career as a Web developer where I developed scalable websites for clients with technologies like HTML, CSS, JavaScript(ReactJS), and PHP. During this time I realized that there were not many articles or tutorials that addressed some complex concepts precisely without much complexity left and in an easier to understand manner. So I set to write my first article which surprisingly got thousands of reads and views within a few months. This set the motivation for me to venture into technical writing. I loved that I can explain complex concepts in and easier to understand manner.

Later in 2022 my professional interest shifted to Data Science and ML which was fueled by writing content which also made made my learning curve faster. Since then I have been able to write engaging content for various companies for their new products  for instance Kangas which is an alternative to Pandas or CometML for model  experiment tracking or release of new LLMs which I have worked on. Those articles have greatly amplified their brands and awareness for those products. 

Technical writing has exposed me to great tech-savvy and subject-matter teams of writers and editors who have helped me master the art of writing engaging, easy to understand, and ranking content. For instance through the use of compelling keywords.

I have been enjoying technical writing and I am especially interested in bringing the experience I have gained to the team at Argon Labs. Since you are focused on helping start-ups, I believe I can venture into any technical content required since what I have learned as a writer is the power of great research and ability to learn fast.

**What are my rates:**

I would normally charge 600 USD for an article This articles tend to be more in-depth and require a lot of research, revision and editing for clarity, accuracy and consistency while maintaining engagement and ranking.

**When can I complete 1 article:**

Depends on the complexity but on average a 1500+ words article could take 2 days. \
 \
**My approach:**



* First I check if the content has been widely covered.
* If covered, I check what additions I can add to it to improve its visibility.
* Then I create an outline.
* Draft the article.
* Revise, edit for clarity, accuracy and consistency.


```
from indexify import IndexifyClient, ExtractionGraph

#initialize the Indexify client
client = IndexifyClient()

# Define the data pipeline
extraction_graph_spec = """
name: 'podcast2knowledgebase'
extraction_policies:
   - extractor: 'tensorlake/whisper-asr'
   name: 'whisperaudioextractor'
   - extractor: 'tensorlake/chunk-extractor'
   name: 'chunker'
   content_source: 'whisperaudioextractor'
   input_params:
       chunk_size: 512
       overlap: 150
   - extractor: 'tensorlake/minilm-l6'
   name: 'minilmembedding'
   content_source: 'chunker'
"""
# Create the extraction graph
extraction_graph = ExtractionGraph.from_yaml(extraction_graph_spec)
client.create_extraction_graph(extraction_graph)

# upload the file to Indexify
content_id = client.upload_file("podcast2knowledgebase",             path="Dr_Cal_Newport_How_to_Enhance_Focus_and_Improve_Productivity.mp3")
print(content_id)











