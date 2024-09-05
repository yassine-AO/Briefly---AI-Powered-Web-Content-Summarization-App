# Briefly: Comprehensive Documentation 🚀

🌐 **Check out the live app at [briefly.uk.to](https://briefly.uk.to)!** 🌐

## Overview

**Briefly** is a web application that scrapes web content and YouTube videos, processes the content using an AI model, and generates summaries. It is built using Flask and advanced web technologies, offering a seamless experience for summarizing text and video content. Below is a technical overview, highlighting key features, architecture, and code snippets.

---

## Table of Contents

1. [Technical Stack](#technical-stack) 🛠️
2. [Application Architecture](#application-architecture) 🏗️
   - [Frontend](#frontend) 🎨
   - [Backend](#backend) 🔧
   - [Web Scraping](#web-scraping) 🕸️
   - [YouTube Summarization](#youtube-summarization) 📺
   - [Download Options](#download-options) 📥
3. [Deployment Technologies](#deployment-technologies) 🚀
4. [Security and Privacy](#security-and-privacy) 🔐
5. [Key Code Snippets](#key-code-snippets) 💻
6. [Conclusion](#conclusion) 🎉

---

## Technical Stack 🛠️

- **Language**: Python 🐍
- **Framework**: Flask 🌶️
- **Web Scraping**: Selenium, BeautifulSoup 🕸️
- **YouTube Summarization**: yt-dlp, LLaMA 3.1-70B via Groq API 📹
- **Frontend**: HTML, CSS, JavaScript (jQuery)
- **Database**: MySQL (SQLAlchemy ORM)
- **Deployment**: AWS EC2, Nginx, Gunicorn 🚀

---

## Application Architecture 🏗️

### Frontend 🎨

- **Bootstrap-based UI**: Responsive and simple to navigate.
- **Interactive Elements**: Engaging features such as a dynamic loading screen and smooth transitions.
- **AJAX Interactions**: Allows asynchronous updates, improving user experience.

### Backend 🔧

The **Flask** backend handles routes, session management, and the integration with the **Groq API** for AI-based summarization.

### Web Scraping 🕸️

Briefly utilizes **Selenium** to render dynamic websites and **BeautifulSoup** to extract relevant content. After scraping, the content is sent to the **Groq API** for summarization using a pre-trained model.

### YouTube Summarization 📺

**B.Watch** processes YouTube video metadata with **yt-dlp** and generates a structured summary using LLaMA 3.1-70B via the Groq API. This allows users to digest video content without watching it.

### Download Options 📥

Summarized content is available for download in multiple formats:
- **PDF**: Generated using **jsPDF**.
- **Text Files**: For simple text summaries.
- **Image Format**: Converted to an image using **html2canvas**.

---

## Deployment Technologies 🚀

- **AWS EC2**: Ensures scalable and reliable hosting.
- **Nginx**: Serves as a reverse proxy, optimizing performance.
- **Gunicorn**: Used for handling requests efficiently.

---

## Security and Privacy 🔐

- **Secure Communication**: The app uses HTTPS for secure data transmission.
- **Logging & Monitoring**: SQLAlchemy logs user interactions, providing insights into app usage while maintaining privacy.

---

## Key Code Snippets 💻

### 1. **YouTube Metadata Extraction**

This function extracts metadata like title, description, views, and transcript from YouTube videos using **yt-dlp**.

```python
def get_youtube_metadata(youtube_url):
    ydl_opts = {'format': 'bestaudio/best', 'quiet': True, 'noplaylist': True}
    with yt_dlp.YoutubeDL(ydl_opts) as ydl:
        info = ydl.extract_info(youtube_url, download=False)
    
    return {
        'title': info.get('title'),
        'description': info.get('description'),
        'views': info.get('view_count'),
        'transcript': extract_transcript(info)  # Fetch transcript if available
    }
```

### 2. **Scraping Web Content**

The function uses **Selenium** to scrape content from websites dynamically.

```python
def scrape_content(url):
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
    driver.get(url)
    soup = BeautifulSoup(driver.page_source, 'html.parser')
    driver.quit()
    return soup.get_text()
```

### 3. **Summarizing Content with LLaMA via Groq API**

This function takes scraped content and generates a structured summary using the **Groq API** and LLaMA model.

```python
def summarize_with_llama(content):
    prompt = (
        "Analyze the content and generate a concise summary. Ensure the summary is structured, capturing key points.\n\n"
        f"Content: {content}"
    )
    response = groq_chat.predict(prompt)
    return response
```

### 4. **Summarizing YouTube Video Metadata**

The metadata of a YouTube video is summarized using the LLaMA model via the **Groq API**.

```python
def summarize_youtube_metadata(meta_data):
    summary_prompt = f"Summarize the YouTube video based on the following metadata:\n{meta_data}"
    return groq_chat.predict(summary_prompt)
```

---

## Conclusion 🎉

**Briefly** is a robust, AI-driven summarization tool for both web content and YouTube videos. Built on a modern tech stack and hosted on AWS, it provides a reliable, fast, and user-friendly interface for generating summaries across different content types.

With its powerful backend integration and advanced AI model, Briefly stands out as a comprehensive solution for content summarization, catering to both casual users and professionals alike.

