## Lab: Integrating API Data into WordPress

**Goal:** Fetch data from an external API (FavQs) and display it on a WordPress page using a shortcode, compatible with the Gutenberg editor.

## Step 1: Install and Activate the Code Snippets Plugin

1.  Go to your WordPress dashboard, navigate to **Plugins > Add New**.
2.  Search for "Code Snippets," install, and activate it.

## Step 2: Add a New Snippet

1.  In your WordPress admin menu, go to **Snippets > Add New**.
2.  Give your snippet a title (e.g., "Display FavQs Quote").
3.  Click on the **Functions (PHP)** tab
4.  Copy and paste the following PHP code into the text box supplied:

    ```php
    function display_favqs_quote_gutenberg() {
        $api_data = do_shortcode("[wpgetapi_endpoint api_id='fav_q' endpoint_id='fav_q_endpoint' raw='true']");
    
        // Decode the JSON data
        $quote_data = json_decode($api_data, true);
    
        if (is_array($quote_data) && isset($quote_data['quote']['body'])) {
            $quote_text = $quote_data['quote']['body'];
            $author = $quote_data['quote']['author']; // Get the author
    
            // Output the quote within a styled block (using a standard paragraph and cite)
            $output = "<div class='wp-block-quote'>";
            $output .= "<p>$quote_text</p>";
            $output .= "<cite>&mdash; $author</cite>"; // Display the author
            $output .= "</div>";
    
            return $output;
        } else {
            return "<p>Failed to retrieve quote.</p>";
        }
    }
    add_shortcode('favqs_quote', 'display_favqs_quote_gutenberg');
    ```

5.  Make sure the snippet is set to "Run snippet everywhere" on your site.
6.  Scroll to the bottom of the page and click **Save Changes and Activate**.

## Step 3: Create a WordPress Page and Add the Shortcode

1.  Log in to your WordPress Admin Dashboard.
2.  Navigate to **Pages > Add New**.
3.  Give your page a title (e.g., "Quote of the Day").
4.  In the Gutenberg editor, add a "Shortcode" block.
5.  Enter the shortcode: `[favqs_quote]`
6.  Click **Publish**.

## Step 4: View the Page

* Visit the page you just created.
* You should see the quote of the day and its author, styled as a blockquote.
