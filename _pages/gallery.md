---
layout: single
title: "Gallery"
permalink: /gallery/
author_profile: true
---

<!-- Flickity CSS -->
<link rel="stylesheet" href="https://unpkg.com/flickity@2/dist/flickity.min.css">

<div class="photo-gallery">
  {% for gallery_item in site.gallery %}
    <div class="gallery-section {% cycle 'left', 'right' %}">
      <div class="gallery-content">
        <div class="gallery-info">
          <h2>{{ gallery_item.title }}</h2>
          <p>{{ gallery_item.description }}</p>
          {% if gallery_item.content != "" %}
            <div class="gallery-text">
              {{ gallery_item.content }}
            </div>
          {% endif %}
        </div>
        
        <div class="carousel-container">
          {% if gallery_item.images.size > 1 %}
            <!-- Multiple images: Use carousel -->
            <div class="carousel" data-flickity='{ "cellAlign": "left", "contain": true, "autoPlay": false, "pauseAutoPlayOnHover": false }'>
              {% for image in gallery_item.images %}
                <div class="carousel-cell">
                  <img src="{{ image | absolute_url }}" alt="{{ gallery_item.title }}" />
                </div>
              {% endfor %}
            </div>
          {% else %}
            <!-- Single image: Just display it -->
            <div class="single-image">
              {% for image in gallery_item.images %}
                <img src="{{ image | absolute_url }}" alt="{{ gallery_item.title }}" />
              {% endfor %}
            </div>
          {% endif %}
        </div>
      </div>
    </div>
  {% endfor %}
</div>

<style>
.carousel-container {
  flex: 1;
  max-width: 500px;
}

.carousel {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.carousel-cell {
  width: 100%;
  height: 300px;
  margin-right: 10px;
  background: #f8f8f8;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.carousel-cell img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}

/* Single image styling - match carousel dimensions */
.single-image {
  width: 100%;
  height: 300px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  justify-content: center;
}

.single-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}

.gallery-section {
  margin-bottom: 60px;
  border-bottom: 1px solid #eee;
  padding-bottom: 40px;
}

.gallery-content {
  display: flex;
  align-items: flex-start;
  gap: 40px;
}

.gallery-info {
  flex: 1;
  min-width: 300px;
}

.gallery-section.left .gallery-content {
  flex-direction: row;
}

.gallery-section.right .gallery-content {
  flex-direction: row-reverse;
}

.gallery-text {
  margin-top: 20px;
  font-size: 1.1em;
  line-height: 1.6;
}

/* Flickity custom styling */
.flickity-page-dots {
  bottom: -30px;
}

.flickity-page-dots .dot {
  width: 12px;
  height: 12px;
  background: #bbb;
  border-radius: 50%;
  margin: 0 5px;
}

.flickity-page-dots .dot.is-selected {
  background: #333;
}

.flickity-prev-next-button {
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border-radius: 50%;
  width: 40px;
  height: 40px;
}

.flickity-prev-next-button:hover {
  background: rgba(0, 0, 0, 0.8);
}

/* Mobile responsive fixes */
@media (max-width: 768px) {
  .gallery-content {
    flex-direction: column !important;
  }
  
  .gallery-info {
    min-width: auto;
  }
  
  .carousel-container {
    max-width: 100%;
    width: 100%;
  }
  
  .carousel {
    height: 250px;
  }
  
  .carousel-cell {
    height: 250px;
    margin-right: 5px;
  }
  
  .single-image {
    height: 250px;
  }
  
  .flickity-prev-next-button {
    width: 35px;
    height: 35px;
  }
  
  .flickity-page-dots .dot {
    width: 14px;
    height: 14px;
    margin: 0 8px;
  }
}

@media (max-width: 480px) {
  .carousel {
    height: 200px;
  }
  
  .carousel-cell {
    height: 200px;
  }
  
  .single-image {
    height: 200px;
  }
  
  .gallery-content {
    gap: 20px;
  }
  
  .gallery-section {
    margin-bottom: 40px;
  }
  
  .flickity-prev-next-button {
    width: 30px;
    height: 30px;
  }
}
</style>

<!-- Flickity JS -->
<script src="https://unpkg.com/flickity@2/dist/flickity.pkgd.min.js"></script>

<script>
// Robust initialization for Jekyll
function initFlickity() {
  console.log('🚀 Initializing Flickity...');
  console.log('Flickity available:', typeof Flickity !== 'undefined');
  console.log('Carousels found:', document.querySelectorAll('.carousel').length);
  
  const carousels = document.querySelectorAll('.carousel');
  
  if (typeof Flickity === 'undefined') {
    console.log('⏳ Flickity not ready, retrying...');
    setTimeout(initFlickity, 200);
    return;
  }
  
  if (carousels.length === 0) {
    console.log('⏳ No carousels found, retrying...');
    setTimeout(initFlickity, 200);
    return;
  }
  
  carousels.forEach(function(carousel, index) {
    // Skip if already initialized
    if (carousel.flickityData) {
      console.log(`✅ Carousel ${index + 1} already initialized`);
      return;
    }
    
    try {
      const flkty = new Flickity(carousel, {
        cellAlign: 'left',
        contain: true,
        autoPlay: false,
        pauseAutoPlayOnHover: false
      });
      
      console.log(`✅ Carousel ${index + 1} initialized successfully`);
      
      // Add hover autoplay for non-touch devices only
      if (!('ontouchstart' in window)) {
        carousel.addEventListener('mouseenter', function() {
          flkty.options.autoPlay = 2000;
          flkty.playPlayer();
        });
        
        carousel.addEventListener('mouseleave', function() {
          flkty.pausePlayer();
          flkty.options.autoPlay = false;
        });
      }
      
    } catch (error) {
      console.error(`❌ Error initializing carousel ${index + 1}:`, error);
    }
  });
}

// Multiple initialization attempts
document.addEventListener('DOMContentLoaded', initFlickity);
window.addEventListener('load', initFlickity);
setTimeout(initFlickity, 1000); // Jekyll fallback
setTimeout(initFlickity, 2000); // Final fallback
</script>