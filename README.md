# Nitrogen
---

Nitrogen is a Python module designed to make desktop app development in Python easier. It utilizes [Flask](https://github.com/pallets/flask), [Jinja2](https://github.com/pallets/jinja), and [web technologies](https://en.wikibooks.org/wiki/Introduction_to_Information_Technology/Web_Technologies) to create the Python equivalent of [Electron](https://github.com/electron/electron).

### Installation

To install from PyPi **(recommended)**:
```sh
python3 -m pip install nitrogenfw
```
or, to install from source:
```sh
git clone https://github.com/iiPythonx/nitrogen
cd nitrogen
python3 -m pip install .
```

### Module usage

To begin developing using Nitrogen, create a `src` folder and a file named `app.py`. The `src` folder is where all your web files will be stored, this includes your HTML, CSS, etc. The `app.py` file will be the entrypoint to your application.  

Inside `app.py`, put the following template:
```py
from nitrogen import Nitrogen
app = Nitrogen(source_dir = "src", use_jinja = True)
app.start()
```

Now, inside `src/index.html`, put any HTML that you want to render. In my case, I put the following:
```html
<div style = "text-align: center; padding-top: 45px">
    <h3>Hello, world!</h3>
</div>
```

To start your app, simply run `app.py`. You should see your HTML file render!

### Configuration

By default, Nitrogen does not need passed any kwargs at runtime. However they can be useful if Nitrogen does not do what you are trying to accomplish.  

When creating a `Nitrogen` object, you can pass the following kwargs:
+ `source_dir`; default: "src"; This is where Nitrogen + Jinja will look for files
+ `use_jinja`; default: True; Indicates if Nitrogen should use Jinja to render templates

After you make the `Nitrogen` object, and you use `app.start()`, you can pass the following:
+ `start_location`; default: "index.html"; This is the first page Nitrogen will render upon start
+ `fullscreen`; default: False; Whether or not to automatically fullscreen the Qt5 window

### Custom  Functionality

Since Nitrogen uses Flask and Socket.io internally, it is possible to overwrite globals, routes, etc.  
To do so, you can reference `Nitrogen.route` and others, like so:
```py
from nitrogen import Nitrogen
app = Nitrogen(source_dir = "src", use_jinja = True)

# The Nitrogen class has .route() as a convenience wrapper
# around the Flask apps .route function.
@app.route("/something")
def something():
    return "hello, this is something."

# Alternatively, you can add socket.io methods:
@app.on("someevent")
def dosomething(a, b, c):
    print(a, b, c)
    app.emit("didsomething")

# To handle this in JS, you use the emit() and on() methods
# that are automatically included in your HTML templates.

# emit(event, ...args)
# on(event, callback)

app.start()
```

You can also customize the Flask app options.
