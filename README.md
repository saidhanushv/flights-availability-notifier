# Flight Price Monitoring Program✈️

This program helps you monitor flight prices from a predefined list of destinations and notifies you when the price drops below a certain threshold. It uses the Tequila API from Kiwi.com to check for available flights and the Twilio API for sending notifications.

## Requirements

- Python 3.x
- Ensure you have the required libraries installed by running: `pip install requests twilio`

## Setup

1. Clone the repository to your local machine.

2. Set up environment variables:
   - `TEQUILA_API_KEY`: Obtain your API key from [Kiwi.com](https://tequila.kiwi.com/portal/login) and set it as an environment variable.
   - `TWILIO_SID`: Sign up for a Twilio account at [Twilio](https://www.twilio.com/try-twilio) and get your SID.
   - `TWILIO_AUTH_TOKEN`: Obtain your Twilio auth token.
   - `TWILIO_VIRTUAL_NUMBER`: Get a Twilio virtual phone number.
   - `TWILIO_VERIFIED_NUMBER`: Set your verified phone number to receive SMS notifications.
   - `MY_EMAIL`: Set your email address to send email notifications.
   - `MY_PASSWORD`: Set your email account password.

3. Create a Google Sheets document with the following columns:
   - `city`: The name of the destination city.
   - `iataCode`: Leave this column empty, it will be populated automatically.

4. Share your Google Sheets document with the email address provided by Twilio in `MY_EMAIL`.

5. Obtain the Sheety API endpoint for the prices data and set it as an environment variable. Create your own endpoint in [Sheety API Dashboard]("https://dashboard.sheety.co/"):
   - `SHEETY_PRICES_ENDPOINT`: The endpoint for the prices data in your Sheety document.
   - `SHEETY_USERS_ENDPOINT`: The endpoint for the user data in your Sheety document.

## Usage

1. Run the `main.py` script using the following command:
    `python main.py`
2. The program will retrieve destination data from the Sheety document. If the `iataCode` column is empty, it will fetch the IATA codes for the cities and update the sheet.

3. The program will then check flight prices for the next six months for each destination. If a flight is found with a price lower than the recorded price in the Google Sheets document, it will notify you via email and/or SMS (Twilio) with the new low price and flight details (or send an email which has been commented).

## How It Works

1. **Data Retrieval:**
- The program starts by fetching destination data from a Google Sheets document using the Sheety API. It reads the list of destinations with their names and corresponding IATA codes (if available) from the sheet.
- If the `iataCode` column is empty for any destination, the program uses the Tequila API from Kiwi.com to get the IATA codes for those destinations based on their city names. These codes are then updated in the Google Sheets document for future reference.

2. **Flight Search:**
- After obtaining the destination data, the program initiates flight searches for each destination using the Tequila API. It looks for flights departing from a specified origin city (in this case, "LON") to the destination city with outbound flights starting from tomorrow and return flights up to six months from today.
- The program searches for round-trip flights and limits the maximum number of stopovers to 0 (direct flights only).
- If the API returns flight data for a destination, the program checks if the price is lower than the recorded lowest price in the Google Sheets document.

3. **Price Comparison and Notification:**
- If a lower-priced flight is found, the program sends notifications to the user.
- For email notifications, the program fetches the email addresses of users from another Google Sheets document that contains customer data. It then sends an email to each user containing details of the cheaper flight, including the price, origin, destination, departure and return dates.
- For SMS notifications, the program uses the Twilio API to send an SMS alert to a verified phone number, notifying the user about the lower-priced flight.

4. **Repeat Checks:**
- The program runs regularly or as per your scheduling preference to check for updated flight prices. If new low-priced flights are found for any destination, it will notify the users accordingly.

That's how the Flight Price Monitoring Program works! It helps you keep track of flight prices for various destinations and ensures you never miss an opportunity to grab a great deal on flights.

## Credits

- This program uses the Tequila API from Kiwi.com for flight search: [Tequila API](https://tequila.kiwi.com/)
- SMS notifications are sent using the Twilio API: [Twilio](https://www.twilio.com/)
- Data management with Google Sheets using the Sheety API: [Sheety API](https://dashboard.sheety.co/).

## Disclaimer

This personal project is only to showcase. Use it responsibly and avoid excessive API calls to avoid potential rate-limiting or blocking from the service providers.

