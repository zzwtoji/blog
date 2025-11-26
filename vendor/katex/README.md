Download KaTeX resources and place them here so the site can load them locally.

Required files:

- katex.min.css
- katex.min.js
- auto-render.min.js

Recommended source (run on a machine with internet access):

curl -L -o katex.min.css "https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css"
curl -L -o katex.min.js "https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"
curl -L -o auto-render.min.js "https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"

After placing these files, restart Hugo (`hugo server -D`) and KaTeX will be loaded from `/vendor/katex/`.
