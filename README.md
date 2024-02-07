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

