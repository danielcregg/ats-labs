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
    $api_url = 'https://favqs.com/api/qotd'; // FavQs API URL
    
    $response = wp_remote_get($api_url);
    
    if (is_wp_error($response)) {
        return '<p>Failed to retrieve quote.</p>';
    }
    
    $body = wp_remote_retrieve_body($response);
    $quote_data = json_decode($body, true);
    
    if (is_array($quote_data) && isset($quote_data['quote']['body'])) {
        $quote_text = esc_html($quote_data['quote']['body']);
        $author = esc_html($quote_data['quote']['author']);
        
        $output = "<div class='wp-block-quote'>";
        $output .= "<p>$quote_text</p>";
        $output .= "<cite>&mdash; $author</cite>";
        $output .= "</div>";
        
        return $output;
    } else {
        return '<p>Failed to retrieve quote.</p>';
    }
}
add_shortcode('favqs_quote', 'display_favqs_quote_gutenberg');
```
6.  Make sure the snippet is set to "Run snippet everywhere" on your site.
7.  Scroll to the bottom of the page and click **Save Changes and Activate**.

## Step 3: Create a WordPress Page and Add the Shortcode

1.  Log in to your WordPress Admin Dashboard.
2.  Navigate to **Pages > Add New**.
3.  Give your page a title (e.g., "Quote of the Day").
4.  Add a "Shortcode" block.
5.  Enter the shortcode: `[favqs_quote]`
6.  Click **Publish**.

## Step 4: View the Page

* Visit the page you just created.
* You should see the quote of the day and its author, styled as a blockquote, Example below:


<div align="center">
  <img src="https://github.com/user-attachments/assets/6c41c1aa-36ae-459a-ac3d-088dc34d87ac"
       alt="Code Snippets Plugin"
       style="border: 10px solid black; padding: 2px; border-radius: 3px;">
</div>

## Step 5: Add your own api on a new page

1. Go to https://github.com/public-apis/public-apis and search for a free open api.
2. Integrate this api into a new page on wordpress. 

