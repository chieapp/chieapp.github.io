Chie is an extensible desktop app for Generative AI like ChatGPT and
New Bing. It works on macOS, Linux and Windows, and uses native UI on each
platform.

<style>
html img.screenshot-dark { display: none }
html.dark img.screenshot-light { display: none }
html.dark img.screenshot-dark { display: block }
</style>

<img src="/introduction/screenshot.png" alt="Screenshot of the Chie app" class="screenshot-light">
<img src="/introduction/screenshot_dark.png" alt="Screenshot of the Chie app" class="screenshot-dark">

## Downloads

<ul>
  <li><a href="https://github.com/chieapp/chie/releases/latest">
      Latest release <span id="version"></span>
  </a></li>
  <li><a href="https://github.com/chieapp/chie/releases">All Releases</a></li>
</ul>

<script>
  fetch('https://api.github.com/repos/chieapp/chie/releases/latest')
    .then(response => response.json())
    .then(data => {
      if (data?.tag_name)
        version.textContent = `(${data.tag_name})`;
    })
</script>
