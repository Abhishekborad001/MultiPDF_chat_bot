# Chat with Multiple PDFs 📚

This project provides an intuitive web application built with Streamlit that allows you to perform question-answering over the content of multiple PDF documents. Leveraging Google's powerful Gemini models and the LangChain framework, it enables you to efficiently extract information and get concise answers from your documents.

## Table of Contents

-   [Introduction](#introduction)
-   [Features](#features)
-   [How It Works](#how-it-works)
-   [Requirements](#requirements)
-   [Installation](#installation)
-   [Environment Setup](#environment-setup)
-   [Usage](#usage)
-   [Project Structure](#project-structure)
-   [Author](#author)
-   [License](#license)

## Introduction

In an age where information is abundant and often siloed in documents, finding specific details can be time-consuming. "MultiPDF_chat_bot" addresses this challenge by transforming your collection of PDFs into a searchable knowledge base. Simply upload your documents, and then ask questions in natural language. The application will process the PDFs, understand your query, and provide relevant answers by referencing the document content.

## Features

*   **Multi-PDF Support:** Upload and process multiple PDF files simultaneously.
*   **Intelligent Text Extraction:** Accurately extracts text content from PDF documents.
*   **Efficient Text Chunking:** Divides large documents into manageable text chunks for optimal processing.
*   **Google Gemini Integration:** Utilizes `models/embedding-001` for generating text embeddings and `gemini-2.0-flash` for conversational AI.
*   **FAISS Vector Store:** Employs FAISS (Facebook AI Similarity Search) for fast and efficient similarity searches to retrieve relevant document sections.
*   **Context-Aware Q&A:** Answers questions based *only* on the provided PDF content, preventing hallucinations.
*   **Streamlit UI:** Provides a clean and interactive web interface for uploading files and asking questions.
*   **Local Storage:** Saves the vector store index locally (`faiss_index`) for quicker access after initial processing.

## How It Works (Behind the Scenes)

This application implements a Retrieval-Augmented Generation (RAG) pattern to deliver accurate answers:

1.  **PDF Upload & Text Extraction:** You upload your PDF files via the Streamlit interface. The application uses `PyPDF2` to read and extract all text from these documents.
2.  **Text Chunking:** The extracted raw text is often very long. To make it manageable for the language model, `RecursiveCharacterTextSplitter` breaks it down into smaller, overlapping chunks.
3.  **Embedding Generation & Vector Store Creation:** Each text chunk is converted into a numerical vector (embedding) using Google's `embedding-001` model. These embeddings are then stored in a `FAISS` vector database. This database allows for quick searches of semantically similar text. The `faiss_index` is saved locally.
4.  **User Question & Similarity Search:** When you ask a question, your question is also converted into an embedding. This embedding is then used to query the FAISS vector store, finding the most relevant text chunks from your PDFs that are semantically similar to your question.
5.  **Contextual Question Answering:** The retrieved relevant text chunks, along with your original question, are fed as context to the `gemini-2.0-flash` conversational model. The model then generates a precise answer based *only* on the provided context.

## Requirements

*   Python 3.8+
*   A Google API Key for accessing Gemini models.

The project dependencies are listed in `requirements.txt`:

```
streamlit
google-generativeai
python-dotenv
langchain
PyPDF2
faiss-cpu
langchain_google_genai
```

## Installation

Follow these steps to set up the project locally:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Abhishekborad001/MultiPDF_chat_bot.git
    cd chat-with-multiple-pdfs
    ```

2.  **Create a virtual environment** (recommended):
    ```bash
    python -m venv venv
    ```

3.  **Activate the virtual environment:**
    *   **On Windows:**
        ```bash
        .\venv\Scripts\activate
        ```
    *   **On macOS/Linux:**
        ```bash
        source venv/bin/activate
        ```

4.  **Install the required packages:**
    ```bash
    pip install -r requirements.txt
    ```

## Environment Setup

You need to obtain a Google API Key to use the Gemini models.

1.  **Get your Google API Key:**
    *   Go to [Google AI Studio](https://aistudio.google.com/app/apikey) or the Google Cloud Console.
    *   Create a new API key. Ensure it has access to the Generative Language API.

2.  **Create a `.env` file:**
    In the root directory of your project (where `main.py` is located), create a file named `.env`.

3.  **Add your API key to the `.env` file:**
    ```
    GOOGLE_API_KEY="your_actual_google_api_key_here"
    ```
    Replace `"your_actual_google_api_key_here"` with the API key you obtained.

## Usage

Once you have completed the installation and environment setup:

1.  **Run the Streamlit application:**
    ```bash
    streamlit run main.py
    ```

2.  **Open the application in your browser:**
    Streamlit will automatically open a new tab in your default web browser, usually at `http://localhost:8501`.

3.  **Interact with the app:**
    *   On the sidebar, use the **"Upload your PDF Files"** button to select one or more PDF documents.
    *   Click the **"Submit & Process"** button. The application will then extract text, chunk it, and create a local FAISS vector store. This might take a few moments depending on the size and number of your PDFs. A "Done" message will appear upon successful processing.
    *   Once processing is complete, you can type your questions into the **"Ask a Question from the PDF Files"** text box and press Enter. The application will retrieve relevant information and provide an answer.

    *(Note: If you close the app and restart it, it will load the `faiss_index` if it exists, so you don't need to re-upload and process PDFs unless you add new ones or want to clear the existing knowledge base.)*

## Project Structure

```
.
├── main.py
├── requirements.txt
├── .env.example (for guidance, you'll create .env)
└── README.md
```

*   **`main.py`**: The main Streamlit application script containing all the core logic for PDF processing, embedding, vector store management, and Q&A.
*   **`requirements.txt`**: Lists all Python dependencies required for the project.
*   **`.env`**: (User-created) Stores your Google API Key. Not committed to version control for security.
*   **`faiss_index/`**: (Generated during runtime) Directory where the FAISS vector store index files are saved.

## Author

*   **[Abhishek Borad]**

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
