# Ways to Improve Web Page Speed

This repository contains strategies and best practices to enhance the loading speed and performance of web pages. Optimizing web page speed is crucial for a better user experience and search engine rankings.

## Techniques to Improve Web Page Speed

### 1. Reduce Image Size and Convert to .webp Format
Large image files significantly contribute to slow loading times. Resize images to appropriate dimensions and compress them. Convert images to the efficient .webp format for improved loading speed without compromising quality.

### 2. Minimize JS and CSS Files
Reduce the number of JavaScript (JS) and Cascading Style Sheets (CSS) files by combining or minifying them. Fewer files mean quicker downloads and parsing by the browser, resulting in faster page rendering.

### 3. Implement Server-Side Caching
Utilize server-side caching mechanisms like HTTP caching headers (e.g., ETags, Last-Modified) and reverse proxies (e.g., Varnish) to store frequently accessed data. This reduces server load and speeds up content delivery to users.
  
    <IfModule mod_expires.c>
        ExpiresActive On
  
      # Images
      ExpiresByType image/jpeg "access plus 1 year"
      ExpiresByType image/gif "access plus 1 year"
      ExpiresByType image/png "access plus 1 year"
      ExpiresByType image/webp "access plus 1 year"
      ExpiresByType image/svg+xml "access plus 1 year"
      ExpiresByType image/x-icon "access plus 1 year"
    
      # Video
      ExpiresByType video/webm "access plus 1 year"
      ExpiresByType video/mp4 "access plus 1 year"
      ExpiresByType video/mpeg "access plus 1 year"
    
      # Fonts
      ExpiresByType font/ttf "access plus 1 year"
      ExpiresByType font/otf "access plus 1 year"
      ExpiresByType font/woff "access plus 1 year"
      ExpiresByType font/woff2 "access plus 1 year"
      ExpiresByType application/font-woff "access plus 1 year"
      ExpiresByType application/font-woff2 "access plus 1 year"
    
      # CSS, JavaScript
      ExpiresByType text/css "access plus 1 year"
      ExpiresByType text/javascript "access plus 1 year"
      ExpiresByType application/javascript "access plus 1 year"
    
      # Others
      ExpiresByType application/pdf "access plus 1 year"
      ExpiresByType image/vnd.microsoft.icon "access plus 1 year"
    </IfModule>

### 4. Serve Compressed Data with Brotli/Gzip
Enable Brotli or Gzip compression to reduce the size of transferred data between the server and the client's browser. Compressed files result in faster downloads and improved overall page speed.
    
    # Requires both mod_rewrite and mod_headers to be enabled.
    <IfModule mod_headers.c>
      # Serve brotli compressed CSS files if they exist and the client accepts gzip.
      RewriteCond %{HTTP:Accept-encoding} br
      RewriteCond %{REQUEST_FILENAME}\.br -s
      RewriteRule ^(.*)\.css $1\.css\.br [QSA]
    
      # Serve gzip compressed CSS files if they exist and the client accepts gzip.
      RewriteCond %{HTTP:Accept-encoding} gzip
      RewriteCond %{REQUEST_FILENAME}\.gz -s
      RewriteRule ^(.*)\.css $1\.css\.gz [QSA]
    
      # Serve brotli compressed JS files if they exist and the client accepts gzip.
      RewriteCond %{HTTP:Accept-encoding} br
      RewriteCond %{REQUEST_FILENAME}\.br -s
      RewriteRule ^(.*)\.js $1\.js\.br [QSA]
    
      # Serve gzip compressed JS files if they exist and the client accepts gzip.
      RewriteCond %{HTTP:Accept-encoding} gzip
      RewriteCond %{REQUEST_FILENAME}\.gz -s
      RewriteRule ^(.*)\.js $1\.js\.gz [QSA]
    
      # Serve correct content types, and prevent mod_deflate double gzip.
      RewriteRule \.css\.gz$ - [T=text/css,E=no-gzip:1]
      RewriteRule \.js\.gz$ - [T=text/javascript,E=no-gzip:1]
      RewriteRule \.css\.br$ - [T=text/css,E=no-gzip:1]
      RewriteRule \.js\.br$ - [T=text/javascript,E=no-gzip:1]
    
      <FilesMatch "(\.js\.gz|\.css\.gz)$">
        # Serve correct encoding type.
        Header set Content-Encoding gzip
        # Force proxies to cache gzipped & non-gzipped css/js files separately.
        Header append Vary Accept-Encoding
      </FilesMatch>
      <FilesMatch "(\.js\.br|\.css\.br)$">
        # Serve correct encoding type.
        Header set Content-Encoding br
        # Force proxies to cache gzipped & non-gzipped css/js files separately.
        Header append Vary Accept-Encoding
      </FilesMatch>
    </IfModule>

### 5. Implement Lazy Loading
Adopt lazy loading for images, iframes, and other non-critical resources. Lazy loading defers the loading of off-screen or below-the-fold elements, allowing the initial page to load faster and then loading these elements as the user scrolls. Use the below code to automatically implement lazy loading to all the image of a webpage
    
    
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
    // Add 'lzy_img' class to all <img> elements
    $('img').addClass('lzy_img');

    // For each <img> element, store the 'src' attribute value in 'data-src'
    $('img').each(function () {
        var srcValue = $(this).attr('src');
        $(this).attr('data-src', srcValue);
    });

    // When the DOM content is loaded
    document.addEventListener("DOMContentLoaded", function () {
        // Create an IntersectionObserver to handle lazy loading
        const imageObserver = new IntersectionObserver((entries, imgObserver) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    // When an image is intersecting the viewport, load its source
                    const lazyImage = entry.target;
                    lazyImage.src = lazyImage.dataset.src;
                }
            });
        });

        // Select all elements with class 'lzy_img'
        const arr = document.querySelectorAll('img.lzy_img');
        
        // Observe each 'lzy_img' element using the IntersectionObserver
        arr.forEach((v) => {
            imageObserver.observe(v);
        });
    });
    </script>


### Tools To check webpage speed
1) https://pagespeed.web.dev/
2) https://gtmetrix.com/
3) https://yellowlab.tools/


