# CurateReads
Overview
CurateReads is a command-line book recommendation bot designed to assist users in discovering books based on their interests. The bot utilizes the Google Books API to fetch book recommendations based on user queries, whether they mention a specific author or inquire about a genre or topic. This bot is an excellent tool for avid readers looking for their next book or for anyone interested in exploring various literary works.

Key Features
1.User Input: Users can input their preferences in a natural language format, such as "mystery books by Agatha Christie" or simply "Agatha Christie," to retrieve relevant recommendations.

2.Book Recommendations: The bot connects to the Google Books API to retrieve and display book recommendations. Each recommendation includes the book title, authors, a brief description, and a link to additional information.

3.Pagination: Users have the option to request more book recommendations, which enhances their exploration of available options.

4.User Interaction: The bot engages users in a dialogue, allowing them to make multiple queries and providing an opportunity to refine their searches based on the initial results.

5.Search Again Option: After displaying recommendations, the bot allows users to conduct a new search easily.

Technical Structure
1. Application Logic
The core functionality of CurateReads revolves around the following components:

Fetching Book Recommendations: The bot communicates with the Google Books API to fetch book data based on user queries. This involves constructing the appropriate API request URL using user input and handling the response.

Displaying Recommendations: The bot formats and prints the book information in a user-friendly way, making it easy for users to read and understand the recommendations.

Author Recognition: The bot identifies mentions of predefined authors in user input and adjusts its API queries accordingly to fetch books by those authors.

2. Code Structure
Below is a detailed breakdown of the code structure and functionality:

import requests
import re

# Function to fetch book recommendations from Google Books API
def recommend_books(query, start_index=0, max_results=5):
    api_key = "YOUR_API_KEY"  # Replace with your actual API key
    url = f"https://www.googleapis.com/books/v1/volumes?q={query}&startIndex={start_index}&maxResults={max_results}&key={api_key}"
    
    response = requests.get(url)
    books = response.json()
    
    recommendations = []
    
    if 'items' in books:
        for item in books['items']:  # Loop through returned books
            book = {
                "title": item['volumeInfo'].get('title', 'No title available'),
                "authors": item['volumeInfo'].get('authors', ['Unknown author']),
                "description": item['volumeInfo'].get('description', 'No description available'),
                "link": item['volumeInfo'].get('infoLink', '#')
            }
            recommendations.append(book)
    else:
        return "No recommendations found."
    
    return recommendations
Explanation of Functions
recommend_books:

Parameters:
query: The search term provided by the user.
start_index: The index to begin fetching results from the API, used for pagination.
max_results: The maximum number of results to return (default is 5).
Functionality:
       -Constructs a URL for the Google Books API.
       -Sends a GET request to fetch data.
       -Parses the JSON response and extracts relevant book information.
       -Returns a list of book recommendations or a message if no recommendations are found.
display_recommendations:

   Parameters: recommendations, which is a list of book dictionaries.
Functionality:
   Iterates through the list of recommendations and prints each book's details, including title, authors, description (truncated), and a 
    link to more information.
extract_author:

This function identifies whether the user input contains the name of a predefined author using regular expressions. It checks the query against a list of known authors and returns the matched author name if found.
3. Main Program Loop
python

def curate_reads_bot():
    print("Welcome to CurateReads, your personalized book recommendation bot!")
    
    while True:
        query = input("What kind of books are you looking for? (e.g., 'mystery by Agatha Christie' or just 'Agatha Christie') ")
        author = extract_author(query)  # Check if an author is mentioned
        
        if author:
            print(f"Fetching all books by {author}...")
            recommendations = recommend_books(f"author:{author}")  # Search for books by author
            display_recommendations(recommendations)
        else:
            start_index = 0  # Keeps track of the current starting point for fetching books
            more = True  # Control whether to keep showing more books

            while more:
                recommendations = recommend_books(query, start_index=start_index)
                display_recommendations(recommendations)

                user_input = input("Do you want more recommendations? (yes/no): ").strip().lower()
                
                if user_input == "yes":
                    start_index += 5  # Increment the start index to fetch the next set of books
                else:
                    more = False  # Exit the inner loop if the user doesn't want more recommendations

        new_search = input("Do you want to search for another type of book? (yes/no): ").strip().lower()
        if new_search != "yes":
            print("Thank you for using CurateReads! Goodbye!")
            break  # Exit the outer loop if the user doesn't want to search again

if __name__ == "__main__":
    curate_reads_bot()
Explanation of the Main Loop
Welcoming the User: The bot greets the user and prompts for their book preferences.

User Query Handling:

The bot captures the user's input and checks for any mentioned authors.
If an author is identified, the bot fetches books specifically by that author.
If no author is found, it fetches recommendations based on the broader query (e.g., genre).
Displaying Recommendations:

Recommendations are displayed to the user with the option to see more results.
The user can continue to request additional recommendations or choose to perform another search.
Loop Control:

The program continues to run until the user decides to exit, providing a seamless experience for exploring book options.
Conclusion
The CurateReads book recommendation bot is a straightforward yet powerful tool for anyone looking to discover new books. By utilizing the Google Books API, it offers users the ability to search for books by specific authors or topics, making it a versatile resource for readers of all tastes. This command-line implementation provides a solid foundation, and future enhancements could include adding a graphical user interface (GUI), machine learning for personalized recommendations, or integration with other book databases.

This bot not only showcases the functionality of interacting with an external API but also highlights the importance of user experience in building engaging applications. With further improvements and features, CurateReads could become a go-to platform for book enthusiasts everywhere
