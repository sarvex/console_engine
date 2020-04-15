# Console Engine

This library provides simple features for handling user's input and display for terminal applications.  
It uses [Termion](https://crates.io/crates/termion) as main tool for handling the screen and inputs. You don't have to worry about initalizing anything because the lib will handle this for you.

# example usage 
```rust
use console_engine::pixel;
use termion::color;
use termion::event::Key;

fn main() {
    // initializes a screen of 20x10 characters with a target of 1 frame per second
    // coordinates will range from [0,0] to [19,9]
    let mut engine = console_engine::ConsoleEngine::init(20, 10, 1);
    let value = 14;
    // main loop, be aware that you'll have to break it because ctrl+C is captured
    loop {
        engine.wait_frame(); // wait for next frame + capture inputs
        engine.clear_screen(); // reset the screen
    
        engine.line(0, 0, 9, 9, pixel::pxl('#')); // draw a line of '#' from [0,0] to [9,9]
        engine.print(0, 4, format!("Result: {}", value)); // prints some value at [0,4]
    
        engine.set_pxl(4, 0, pixel::pxl_fg('O', color::Cyan)); // write a majestic cyan 'O' at [4,0]

        if engine.is_key_pressed(Key::Char('q')) { // if the user presses 'q' :
            break; // exits app
        }
    
        engine.draw(); // draw the screen
    }
}
```