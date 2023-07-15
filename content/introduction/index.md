Chie is an extensible desktop app for large language models like ChatGPT and
New Bing. It works on macOS, Linux and Windows, and uses native UI on each
platform.

<style>
  @media (prefers-color-scheme: light) {
    img.screenshot-dark { display: none }
  }
  @media (prefers-color-scheme: dark) {
    img.screenshot-light { display: none }
  }
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
