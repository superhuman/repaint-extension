Infinite repaint bug reproduction

On Chrome 60 (not Chrome 59) loading an extension iframe causes the page to
continually re-render. even on a trivial example like the one here, this uses
upwards of 5% of the CPU while the page is focused.  On a production app like
https://mail.superhuman.com/ this can be up to 5-20% of the CPU depending on
the complexity of the view.

To reproduce:

```
git clone https://github.com/superhuman/repaint-extension
cd repaint-extension
python -m SimpleHTTPServer 8092
```

Open Chrome:

* Go to chrome://extensions
* Load unpacked extensions
* Open the repaint-extension directory.
* Go to http://localhost:8092
* Go to http://localhost:8092/index2.html

Expected:
* Neither tab uses an appreciable amount of CPU time, and doesn't repaint every frame.

Actual:
* index.html which loads the iframe through an extension repaints every frame
  while the tab is focused.  (as observed by the CPU being pegged at 4% of the
  CPU, and by using the developer tools "Performance" tab)
* index2.html which loads the iframe over http does not exhibit this problem.

