#SENTIMENTAL ANALYSIS
import requests
from bs4 import BeautifulSoup
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer

nltk.download('vader_lexicon')

def get_text_from_url(url):
    # Send a GET request to the URL
    response = requests.get(url)

    # Check if the request was successful (status code 200)
    if response.status_code == 200:
        # Parse the HTML content of the page
        soup = BeautifulSoup(response.text, 'html.parser')

        # Extract text content from the page
        text = ' '.join([p.text for p in soup.find_all('p')])

        return text

    else:
        print(f'Error: {response.status_code}')
        return None

def sentiment_analysis(text):
    # Initialize the VADER sentiment analyzer
    sid = SentimentIntensityAnalyzer()

    # Get the sentiment scores
    sentiment_scores = sid.polarity_scores(text)

    # Display the sentiment scores
    print(f"Text: {text}")
    print(f"Positive Score: {sentiment_scores['pos']}")
    print(f"Negative Score: {sentiment_scores['neg']}")
    print(f"Compound Score: {sentiment_scores['compound']}")

    # Determine the sentiment category based on the compound score
    if sentiment_scores['compound'] >= 0.05:
        print("Sentiment: Positive")
    elif sentiment_scores['compound'] <= -0.05:
        print("Sentiment: Negative")
    else:
        print("Sentiment: Neutral")

# Example usage
url_to_scrape = 'https://insights.blackcoffer.com/rising-it-cities-and-its-impact-on-the-economy-environment-infrastructure-and-city-life-by-the-year-2040-2/'
text_from_url = get_text_from_url(url_to_scrape)

if text_from_url:
    sentiment_analysis(text_from_url)
