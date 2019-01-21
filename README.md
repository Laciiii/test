# Lab Session Guide 

In diesem Guide sind nützliche Tipps, um die LabSession zu meistern :D 

## Agenda 
1. Motivation 
2. Kahoot! 
3. Spiel 
  - Blender
  - Projektmanager  
  - JS 


### Motivation
Zurücklehnen und zuhören :) 

### Kahoot!
Der Kahoot Code wird in der LabSession generiert.  

### Spiel
Wir möchten ein kleines Spiel in Blend4web erstellen, um die Funktionalität darzustellen. 

Vorbereitung: 
- Öffne Blender 
- Öffne einen TextEditor deiner Wahl (Notepad++, Visual Studio Code, etc.)

Taks: 
- Blender Szene erstellen 
- Blend4web Projekt mit Hilfe des Projektmanagers erstellen 
- Text ausgabe 
- "Player" scripten 
- "Bälle" scripten
- Spiel Logik 

### Blender 

- Starte mit einer leeren Szene und verändere den Start-Würfel 
- Erstelle eine Ebene auf dem der Würfel stehen kann 
- Erstelle eine Sphäre  

### Projektmanger 

Erstelle ein neues Projekt, vergiss dabei nicht das deine Blender Files noch nicht eingebunden sind. 

### JS 

module die benötigt werden 
```javascript
var m_app       = require("app");
var m_cfg       = require("config");
var m_data      = require("data");
var m_preloader = require("preloader");
var m_ver       = require("version");
var m_scenes    = require("scenes");
var m_trans     = require("transform");
var m_ctl       = require("controls");
var m_obj       = require("objects");
```
Jetzt die Anzeige
```javascript
function create_text_display() {
        var left_div = document.createElement("div");
        left_div.id = "left_div_id";
        left_div.style.position = "relative";
        left_div.style.color = "#800000";
        left_div.style.fontSize = "30px";
        left_div.style.fontFamily = "sans-serif";
        left_div.style.fontWeight = "900";
        left_div.innerHTML = "Achtung " + BALL + " Bälle kommen";
        document.body.appendChild(left_div);
    }
```

Der Spieler soll sich nun bewegen 
```javascript
function movePlayer(){
        
        var player = m_scenes.get_object_by_name("Player");

        var player_trans = m_trans.get_translation(player);
        

        var movePlayer_cb = function(obj, id, pulse) {

            var ellapsed = m_ctl.get_sensor_value(obj, id, 2);

            var changeX = -STEP*ellapsed;
    
            switch (id) {
            case "LEFT":
                var left = m_ctl.get_sensor_value(obj, id, 0)
                        || m_ctl.get_sensor_value(obj, id, 1);
                if (left)
                    player_trans[1] += changeX;
                break;
            case "RIGHT":
                //enter your code here 
                break;
            }
    
            // what to do now ? 
                
        }



        create_sensors(player, movePlayer_cb);
    }
```

```javascript
function makeDup() {
    
        var rand_y;
        var src_obj = m_scenes.get_object_by_name("Sphere");
        var Sphere_num = 1;
        var myVar = setInterval(myTimer, 3000);
        setTimeout(function(){if (Sphere_num = BALL) _game_won = true}, BALL*3000 + 5000);
    
        function myTimer() {
            if (_game_lost || _game_won) {
                clearInterval(myVar);

                if (_game_lost)
                    document.getElementById("left_div_id").innerHTML = "Du hast verloren!"
                else
                    document.getElementById("left_div_id").innerHTML = "Gut gemacht, du hast " + BALL + " Bälle abgewhert"
            }
            else {
                if (Sphere_num <= BALL) {
                    document.getElementById("left_div_id").innerHTML = " Bälle kommen: " + Sphere_num;
                    
                    // TODO 
                    // initialisieren der Bälle 

                    setTimeout(function(){if (m_trans.get_translation(Sphere)[2] < 0) _game_lost = true}, 2000);
                    setTimeout(function(){ m_scenes.remove_object(Sphere)}, 2000);

                    Sphere_num++;
                }
            }
        }
    }
```




.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
Lösung 
Schau dir die Lösung nur an wenn du nicht weiterkommst !!!
```javascript
"use strict"

// register the application module
b4w.register("test_game3_main", function(exports, require) {

// import modules used by the app
var m_app       = require("app");
var m_cfg       = require("config");
var m_data      = require("data");
var m_preloader = require("preloader");
var m_ver       = require("version");
var m_scenes    = require("scenes");
var m_trans     = require("transform");
var m_ctl       = require("controls");
var m_obj       = require("objects");

var m_mat       = require("material");
var m_cont    = b4w.container;
var m_mouse   = b4w.mouse;
var m_rgba      = b4w.rgba ;
var m_sfx   = require("sfx");

// detect application mode
var DEBUG = (m_ver.type() == "DEBUG");

// automatically detect assets path
var APP_ASSETS_PATH = m_cfg.get_assets_path("test_game3");

var STEP = 5;

var BALL = 2; 

var _game_lost = false;
var _game_won = false; 



/**
 * export the method to initialize the app (called at the bottom of this file)
 */
exports.init = function() {
    m_app.init({
        canvas_container_id: "main_canvas_container",
       
        callback: init_cb,
        show_fps: DEBUG,
        console_verbose: DEBUG,
        autoresize: true
    });
}

/**
 * callback executed when the app is initialized 
 */
function init_cb(canvas_elem, success) {

    if (!success) {
        console.log("b4w init failure");
        return;
    }

    m_preloader.create_preloader();

    // ignore right-click on the canvas element
    canvas_elem.oncontextmenu = function(e) {
        e.preventDefault();
        e.stopPropagation();
        return false;
    };

    load();
}

/**
 * load the scene data
 */
function load() {
    m_data.load(APP_ASSETS_PATH + "test_game3.json", load_cb, preloader_cb);
}

/**
 * update the app's preloader
 */
function preloader_cb(percentage) {
    m_preloader.update_preloader(percentage);
}

/**
 * callback executed when the scene data is loaded
 */
function load_cb(data_id, success) {

    if (!success) {
        console.log("b4w load failure");
        return;
    }

    // place your code here
    
    
    create_text_display();

    movePlayer(); 

    makeDup(); 


    function create_text_display() {
        var left_div = document.createElement("div");
        left_div.id = "left_div_id";
        left_div.style.position = "relative";
        left_div.style.color = "#800000";
        left_div.style.fontSize = "30px";
        left_div.style.fontFamily = "sans-serif";
        left_div.style.fontWeight = "900";
        left_div.innerHTML = "Achtung " + BALL + " Bälle kommen";
        document.body.appendChild(left_div);
    }


    function makeDup() {
    
        var rand_y;
        var src_obj = m_scenes.get_object_by_name("Sphere");
        var Sphere_num = 1;
        var myVar = setInterval(myTimer, 3000);
        setTimeout(function(){if (Sphere_num = BALL) _game_won = true}, BALL*3000 + 5000);
    
        function myTimer() {
            if (_game_lost || _game_won) {
                clearInterval(myVar);

                if (_game_lost)
                    document.getElementById("left_div_id").innerHTML = "Du hast verloren!"
                else
                    document.getElementById("left_div_id").innerHTML = "Gut gemacht, du hast " + BALL + " Bälle abgewhert"
            }
            else {
                if (Sphere_num <= BALL) {
                    document.getElementById("left_div_id").innerHTML = " Bälle kommen: " + Sphere_num;
                    
                    var Sphere = m_obj.copy(src_obj, "Sphere" + Sphere_num.toString(), false);
                    
                    rand_y = 6 * Math.random();

                    m_trans.set_translation(Sphere, 0.851181, rand_y-3, 10);

                    m_scenes.append_object(Sphere);


                    setTimeout(function(){if (m_trans.get_translation(Sphere)[2] < 0) _game_lost = true}, 2000);
                    setTimeout(function(){ m_scenes.remove_object(Sphere)}, 2000);

                    Sphere_num++;
                }
            }
        }
    }



    function movePlayer(){
        
        var player = m_scenes.get_object_by_name("Player");

        var player_trans = m_trans.get_translation(player);
        

        var movePlayer_cb = function(obj, id, pulse) {

            var ellapsed = m_ctl.get_sensor_value(obj, id, 2);

            var changeX = -STEP*ellapsed;
    
            switch (id) {
            case "LEFT":
                var left = m_ctl.get_sensor_value(obj, id, 0)
                        || m_ctl.get_sensor_value(obj, id, 1);
                if (left)
                    player_trans[1] += changeX;
                break;
            case "RIGHT":
                var right = m_ctl.get_sensor_value(obj, id, 0) 
                        || m_ctl.get_sensor_value(obj, id, 1);
                if (right)
                    player_trans[1] -= changeX;
                break;
            }
    
            if (left || right)
            console.log("move")
                m_trans.set_translation_v(obj, player_trans);
                
        }



        create_sensors(player, movePlayer_cb);
    }


    function create_sensors(player, movePlayer_cb) {
        var key_d = m_ctl.create_keyboard_sensor(m_ctl.KEY_D);
        var key_a = m_ctl.create_keyboard_sensor(m_ctl.KEY_A);

        var key_right = m_ctl.create_keyboard_sensor(m_ctl.KEY_RIGHT);
        var key_left = m_ctl.create_keyboard_sensor(m_ctl.KEY_LEFT);

        var e_s = m_ctl.create_elapsed_sensor();
    
        m_ctl.create_sensor_manifold(player, "LEFT", m_ctl.CT_CONTINUOUS,
                [key_a, key_left, e_s],
                function() {return true}, movePlayer_cb);
                
        m_ctl.create_sensor_manifold(player, "RIGHT", m_ctl.CT_CONTINUOUS,
                [key_d, key_right, e_s],
                function() {return true}, movePlayer_cb);
    
    }



}


});

// import the app module and start the app by calling the init method
b4w.require("test_game3_main").init();

```










