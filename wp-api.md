## **Lab: Integrating OpenWeather API with WordPress**  
**Goal:** Fetch weather data from the OpenWeather API and display it on a WordPress page using a shortcode.  

### **Prerequisites:**  
- Access to the WordPress admin dashboard  
- SSH access to the WordPress server  
- A code editor (VS Code, Nano, Vim, etc.)  
- A free [OpenWeather API key](https://home.openweathermap.org/users/sign_up)  

---

## **Step 1: Get an OpenWeather API Key**  
1. Go to [OpenWeather’s website](https://home.openweathermap.org/users/sign_up).  
2. Sign up for a free account.  
3. After verification, navigate to **API keys** in your profile.  
4. Copy the generated API key.  

---

## **Step 2: Connect to Your WordPress Server via SSH**  
1. Open a terminal on your local machine.  
2. Connect to your WordPress server using SSH:  
   ```bash
   ssh username@your-server-ip
   ```  
3. Navigate to your theme’s directory:  
   ```bash
   cd /var/www/html/wp-content/themes/your-theme/
   ```  
4. Open `functions.php` for editing:  
   ```bash
   nano functions.php
   ```  

---

## **Step 3: Create a Function to Fetch Weather Data**  
Add the following code to `functions.php`:  

```php
function get_weather_data() {
    $city = "Galway";
    $api_key = "YOUR_API_KEY_HERE";
    $api_url = "https://api.openweathermap.org/data/2.5/weather?q=$city&units=metric&appid=$api_key";

    $response = wp_remote_get($api_url);

    if (is_wp_error($response)) {
        return "Error fetching weather data.";
    }

    $body = wp_remote_retrieve_body($response);
    $data = json_decode($body, true);

    if (!$data || !isset($data['main'])) {
        return "Invalid response from API.";
    }

    $temp = $data['main']['temp'];
    $weather = $data['weather'][0]['description'];

    return "<div class='weather-widget'>
                <p><strong>Weather in $city:</strong></p>
                <p>Temperature: $temp°C</p>
                <p>Condition: $weather</p>
            </div>";
}

function weather_shortcode() {
    return get_weather_data();
}

add_shortcode('weather', 'weather_shortcode');
```

📌 **Replace `YOUR_API_KEY_HERE` with your actual API key.**  

---

## **Step 4: Clear Cache and Save Changes**  
1. Save the file in Nano (`CTRL + X`, then `Y`, then `Enter`).  
2. If you have a caching plugin, clear the cache.  

---

## **Step 5: Add the Weather Widget to a WordPress Page**  
1. Log in to your **WordPress Admin Dashboard**.  
2. Navigate to **Pages > Add New**.  
3. Give your page a title (e.g., "Galway Weather").  
4. In the content editor, add the shortcode:  
   ```
   [weather]
   ```  
5. Click **Publish**.  

---

## **Step 6: View the Weather Widget**  
- Visit the page you just created.  
- You should see **Galway’s current weather, temperature, and conditions**.  

---

## **Extensions & Enhancements**  
🔹 **Cache API Response:** Store results in a transient to reduce API calls.  
🔹 **Style with CSS:** Add custom styling to `.weather-widget`.  
🔹 **Live Updates with JavaScript:** Use AJAX to refresh data every minute.  

---

**Congratulations! 🎉 Your WordPress site now displays real-time weather data for Galway!**
