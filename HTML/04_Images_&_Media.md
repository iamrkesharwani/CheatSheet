# HTML Images & Media `v5.0`

## Essentials

```html
<!-- Basic image -->
<img src="photo.jpg" alt="Descriptive text" width="300" height="200">

<!-- Responsive image -->
<img src="photo-800w.jpg" 
     srcset="photo-400w.jpg 400w, photo-800w.jpg 800w, photo-1200w.jpg 1200w"
     sizes="(max-width: 600px) 100vw, 50vw"
     alt="Responsive photo" loading="lazy">

<!-- Video with fallbacks -->
<video controls width="640" height="360" poster="thumbnail.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    Your browser doesn't support video.
</video>

<!-- Audio with multiple formats -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    Your browser doesn't support audio.
</audio>
```

**Key Concepts**: Responsive Images | Format Fallbacks | Accessibility | Performance Optimization

**When to use**: Displaying visual content, media playback, and creating responsive multimedia experiences

## Syntax Reference

### Image Formats & Use Cases

```html
<!-- JPEG - photos with many colors -->
<img src="photo.jpg" alt="Family vacation">

<!-- PNG - logos, graphics, transparency -->
<img src="logo.png" alt="Company logo">

<!-- SVG - scalable icons and graphics -->
<img src="icon.svg" alt="Settings icon">

<!-- WebP - modern compressed format -->
<img src="optimized.webp" alt="High-quality compressed image">

<!-- GIF - simple animations -->
<img src="loading.gif" alt="Loading animation">
```

### Responsive Images

```html
<!-- Srcset for different screen sizes -->
<img src="default.jpg"
     srcset="small.jpg 400w, medium.jpg 800w, large.jpg 1200w"
     sizes="(max-width: 600px) 100vw, (max-width: 1000px) 80vw, 1200px"
     alt="Responsive image">

<!-- High-DPI displays -->
<img src="icon.png" 
     srcset="icon.png 1x, icon@2x.png 2x, icon@3x.png 3x" 
     alt="High-DPI icon">

<!-- Picture element for art direction -->
<picture>
    <source media="(min-width: 800px)" srcset="wide.jpg">
    <source media="(min-width: 400px)" srcset="medium.jpg">
    <img src="narrow.jpg" alt="Art-directed image">
</picture>

<!-- Format fallbacks -->
<picture>
    <source srcset="image.webp" type="image/webp">
    <source srcset="image.avif" type="image/avif">
    <img src="image.jpg" alt="Modern format with fallback">
</picture>
```

### Video Implementation

```html
<!-- Basic video with controls -->
<video controls width="640" height="360">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    Fallback text for unsupported browsers.
</video>

<!-- Video with all attributes -->
<video controls autoplay muted loop playsinline 
       preload="metadata" poster="poster.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
</video>

<!-- Video with subtitles -->
<video controls>
    <source src="movie.mp4" type="video/mp4">
    <track kind="subtitles" src="en.vtt" srclang="en" label="English" default>
    <track kind="subtitles" src="es.vtt" srclang="es" label="Español">
    <track kind="captions" src="captions.vtt" srclang="en" label="English CC">
</video>
```

### Audio Implementation

```html
<!-- Basic audio player -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <source src="audio.wav" type="audio/wav">
    Your browser doesn't support audio.
</audio>

<!-- Background audio -->
<audio autoplay muted loop>
    <source src="ambient.mp3" type="audio/mpeg">
</audio>

<!-- Audio with chapters -->
<audio controls>
    <source src="podcast.mp3" type="audio/mpeg">
    <track kind="chapters" src="chapters.vtt" srclang="en" default>
</audio>
```

### Semantic Structure

```html
<!-- Figure with caption -->
<figure>
    <img src="chart.png" alt="Sales data visualization">
    <figcaption>Q4 2024 sales showing 25% growth</figcaption>
</figure>

<!-- Multiple images in figure -->
<figure>
    <img src="before.jpg" alt="Room before renovation">
    <img src="after.jpg" alt="Room after renovation">
    <figcaption>Kitchen makeover: before and after</figcaption>
</figure>
```

### Image Maps

```html
<!-- Interactive clickable areas -->
<img src="map.jpg" alt="World map" usemap="#worldmap">
<map name="worldmap">
    <area shape="rect" coords="0,0,100,100" href="/region1" alt="North America">
    <area shape="circle" coords="200,150,50" href="/region2" alt="Europe">
    <area shape="poly" coords="300,200,400,250,350,300" href="/region3" alt="Asia">
</map>
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Include descriptive alt text for screen readers | Use generic alt text like "image" or "photo" |
| Specify width/height to prevent layout shifts | Skip dimensions causing content jumps |
| Use appropriate formats (JPEG for photos, PNG for graphics) | Use wrong format increasing file sizes |
| Implement lazy loading for below-fold images | Lazy load above-fold content |
| Provide multiple video/audio formats | Rely on single format excluding users |
| Use figure/figcaption for semantic meaning | Use div/span for image captions |

## Quick Fixes

- **Image not showing** → Check file path, permissions, and format support
- **Responsive images not switching** → Verify srcset syntax and sizes attribute
- **Video autoplay blocked** → Add `muted` attribute for autoplay to work
- **Audio not playing** → Include multiple format sources for compatibility
- **Layout shifting during load** → Add width/height attributes to images
- **Accessibility issues** → Ensure alt text describes image content, not filename

## Gotchas

⚠️ **Autoplay Policy**: Modern browsers block autoplay without user interaction unless video is muted

⚠️ **Loading Attribute**: `loading="eager"` is default for above-fold images; use `loading="lazy"` for performance

⚠️ **Picture Element**: Always include `<img>` as fallback inside `<picture>` element

⚠️ **Alt Text**: Use empty `alt=""` for decorative images, never omit the attribute entirely

⚠️ **Format Support**: WebP and AVIF aren't supported in all browsers - always provide fallbacks

⚠️ **Video Codecs**: Different browsers support different codecs - test across platforms

## Checklist

- [ ] All images have meaningful alt text or empty alt for decorative images
- [ ] Width and height attributes set to prevent layout shifts
- [ ] Responsive images implemented with srcset and sizes for different viewports
- [ ] Modern image formats (WebP/AVIF) with fallbacks for better compression
- [ ] Lazy loading enabled for below-fold images
- [ ] Multiple video/audio formats provided for browser compatibility
- [ ] Video includes poster image for better perceived performance
- [ ] Captions/subtitles added for accessibility when needed
- [ ] Figure/figcaption used for images requiring descriptions
- [ ] File sizes optimized for web delivery without quality loss