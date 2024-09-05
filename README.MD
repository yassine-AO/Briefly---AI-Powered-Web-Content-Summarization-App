# Briefly: Comprehensive Documentation 🚀

🌐 **Check out the live app at [briefly.uk.to](https://briefly.uk.to)!** 🌐

## Overview

**Briefly** is a web app designed for efficient web and YouTube content summarization, leveraging the Flask framework and integrating with an advanced AI language model. Below is a detailed overview of its architecture, deployment, and key features, with streamlined code snippets from the actual implementation.

---

## Table of Contents

1. [Technical Stack](#technical-stack)
2. [Application Architecture](#application-architecture)
   - [Frontend](#frontend)
   - [Backend](#backend)
   - [Web Scraping](#web-scraping)
   - [YouTube Summarization](#youtube-summarization)
   - [Download Options](#download-options)
3. [Deployment Technologies](#deployment-technologies)
4. [Security and Privacy](#security-and-privacy)
5. [Conclusion](#conclusion)

---

## Technical Stack

- **Language**: Python 🐍
- **Framework**: Flask 🌶️
- **Web Scraping**: Selenium, BeautifulSoup 🕸️
- **YouTube Summarization**: yt-dlp, LLaMA 3.1-70B via Groq API 📹
- **Frontend**: HTML, CSS, JavaScript (jQuery)
- **Database**: MySQL (SQLAlchemy ORM)
- **Deployment**: AWS EC2, Nginx, Gunicorn 🚀

---

## Application Architecture

### Frontend

The **Frontend** leverages Bootstrap for responsiveness and jQuery for seamless interactions. Users can input website links or YouTube URLs, with dynamic text effects for an engaging experience.

### Backend

The **Backend** is built using Flask, handling routing and request processing. The summarization logic connects with the **Groq API** via **LLMChain**. When processing a URL, the app uses:

```python
@app.route('/summarize', methods=['POST'])
def process_url():
    website_url = request.form['website_url']  # Retrieve the user-submitted URL
    response_text = logic.scrape_summarize(website_url)  # Scrape and summarize
    return jsonify({'response': response_text})
```

### Web Scraping

For **Web Scraping**, the app utilizes **Selenium** to handle dynamic web content and **BeautifulSoup** to parse the HTML. This ensures that only relevant content is extracted, filtering out unnecessary elements such as ads. Below is a simplified version of how the app scrapes content:

```python
def scrape_content(url):
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
    driver.get(url)
    soup = BeautifulSoup(driver.page_source, 'html.parser')  # Parse the HTML
    driver.quit()
    return soup.get_text()  # Return the text content
```

After scraping, the content is summarized using **LLaMA 3.1-70B**, which is integrated with the **Groq API**:

```python
def summarize_with_llama(content):
    prompt = f"Analyze the content and generate a concise summary.\n\nContent:\n{content}"
    return groq_chat.predict(prompt)  # AI-generated summary
```

### YouTube Summarization

The **YouTube Summarization** feature, **B.Watch**, extracts video metadata and transcripts using **yt-dlp**. The video data is then processed similarly to web content for summarization. Here's a snippet of how metadata is extracted:

```python
def get_youtube_metadata(youtube_url):
    with yt_dlp.YoutubeDL(ydl_opts) as ydl:
        info = ydl.extract_info(youtube_url, download=False)
    return {
        'title': info.get('title'),
        'description': info.get('description'),
        'views': info.get('view_count'),
        'transcript': extract_transcript(info)  # Extract transcript
    }
```

Once the metadata is gathered, it is summarized using the same **Groq API**:

```python
def summarize_youtube_metadata(meta_data):
    summary_prompt = f"Summarize the YouTube video based on the following metadata:\n{meta_data}"
    return groq_chat.predict(summary_prompt)
```

### Download Options

The summarized content can be downloaded in various formats, including **PDF**, **plain text**, and **image** formats. The app uses libraries like **jsPDF** and **html2canvas** for generating downloads.

---

## Deployment Technologies

**Briefly** is deployed using **AWS EC2** for reliable hosting and **Nginx** as a reverse proxy for handling traffic efficiently. **Gunicorn** is used as the WSGI server to manage requests.

```python
# Configuration for deployment using AWS, Nginx, and Gunicorn
# AWS EC2 instance setup, Nginx configuration, and systemd service
```

---

## Security and Privacy

All interactions are secure, with data transmitted over HTTPS. The app ensures privacy by not storing any personal information from the users. Session management is handled using secure tokens.

```python
# Session handling with Flask, secure token generation, and HTTPS setup
```

---

## Conclusion

**Briefly** is a powerful and flexible tool for summarizing both web content and YouTube videos. By integrating advanced AI models and using modern web scraping techniques, the app offers a user-friendly experience for professionals and casual users alike. Its deployment on AWS ensures scalability and high availability.
