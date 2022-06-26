[![License][license-badge]][license-link]
[![Online][online-badge]][online-link]

[licence-badge]: https://img.shields.io/github/license/tommct/spikingthesis.svg
[licence-link]: https://github.com/tommct/spikingthesis/blob/master/LICENSE
[online-badge]: https://img.shields.io/badge/read-online-green.svg
[online-link]: https://tommct.github.io/spikingthesis/

# spikingthesis
All resources to compile and publish "A Theory of Spiking Intelligence"

## Building the Book ##

1. Pull the book repository
   ```bash
   git clone https://github.com/tommct/spikingthesis.git

   cd spikingthesis
   ```
2. Install [*Jupyter Book*][jb-pypi] (`requirements.txt`)
   ```bash
   pip install -r requirements.txt
   ```
3. Build the book
   ```bash
   jb build .
   ```
4. Open the html build
   ```bash
   open _build/html/index.html
   ```
   or run it as a server (to get the embedded SlidesLive window to work)
   ```bash
   python -m http.server --directory _build/html
   open http://localhost:8000
   ```
   
[jb-pypi]: https://pypi.org/project/jupyter-book/

# spikingthesis
# spikingthesis
# spikingthesis
# spikingthesis
# spikingthesis
# spikingthesis
