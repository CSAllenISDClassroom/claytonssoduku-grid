import Foundation
import Curses

// Define interrupt handler to terminate Curses with CTRL-C
class Handler : CursesHandlerProtocol {
    func interruptHandler() {
        screen.shutDown()
        print("Good bye!")
        exit(0)
    }
    
    func windowChangedHandler() {
    }
}


// Start up Curses
let screen = Screen.shared
screen.startUp(handler:Handler())
let mainWindow = screen.window
let size = mainWindow.size

// Move the cursor to an absolute position
let cursor = mainWindow.cursor
cursor.position = Point(x:20, y:20)

//Color set up

let colors = Colors.shared
precondition(colors.areSupported, "This terminal doesn't support colors.")
colors.startUp()
let red = Color.standard(.red)
let white = Color.standard(.white)
let redOnWhite = colors.newPair(foreground:red, background:white)
let cyan = Color.standard(.cyan)
let black = Color.standard(.black)
let cyanOnBlack = colors.newPair(foreground:black, background:cyan)

//Functions
var scale = 6
//Function used to create boxes without cliping, it must however be used incomplete.
func LineofBox(X1:Int, Y1:Int, Times:Int) {
    
    let Y2 = Y1 + 1
    let Y3 = Y1 + 2
    var X1 = X1
    let T = Times - 1
    var i = 0
    
    
    mainWindow.cursor.position = Point(x:X1, y:Y1)
    mainWindow.write("├")

    for _ in 1 ... scale {
        mainWindow.write("─")
    }

    
    mainWindow.cursor.position = Point(x:X1, y:Y2)
    mainWindow.write("│")
    
    for _ in 1 ... scale - 1 {
        mainWindow.write(" ")
    }
   
    mainWindow.cursor.position = Point(x:X1, y:Y3)
    mainWindow.write("├")
     for _ in 1 ... scale {
        mainWindow.write("─")
    }
   
   
    X1 = X1 + scale
    for _ in 0 ... T {
        mainWindow.cursor.position = Point(x:X1, y:Y1)
        mainWindow.write("┼")
        for _ in 1 ... scale {
        mainWindow.write("─")
    }
   
        
        mainWindow.cursor.position = Point(x:X1, y:Y2)
        mainWindow.write("│")
            for _ in 1 ... scale - 1 {
        mainWindow.write(" ")
    }

        mainWindow.cursor.position = Point(x:X1, y:Y3)
        mainWindow.write("┼")
        for _ in 1 ... scale {
        mainWindow.write("─")
    }
   
        X1 = X1 + scale

        i = i + 1
        
    }

    mainWindow.cursor.position = Point(x:X1, y:Y1)
    mainWindow.write("┤")
    mainWindow.cursor.position = Point(x:X1, y:Y2)
    mainWindow.write("│")
    mainWindow.cursor.position = Point(x:X1, y:Y3)
    mainWindow.write("┤")    
}

func TopLineBox(X1:Int, Y1:Int, Times:Int) {
    
    let Y2 = Y1 + 1
    let Y3 = Y1 + 2
    var X1 = X1
    let T = Times - 1
    var i = 0
    
    mainWindow.cursor.position = Point(x:X1, y:Y1)
    mainWindow.write("┌")
    for _ in 1 ... scale {
        mainWindow.write("─")
    }
    mainWindow.cursor.position = Point(x:X1, y:Y2)
    mainWindow.write("│")
    for _ in 1 ... scale - 1 {
        mainWindow.write(" ")
    }
    mainWindow.cursor.position = Point(x:X1, y:Y3)
    mainWindow.write("├")
    for _ in 1 ... scale {
        mainWindow.write("─")
    }
    X1 = X1 + scale
    for _ in 0 ... T {
        mainWindow.cursor.position = Point(x:X1, y:Y1)
        mainWindow.write("┬")
        for _ in 1 ... scale {
        mainWindow.write("─")
    }
        mainWindow.cursor.position = Point(x:X1, y:Y2)
        mainWindow.write("│")
        for _ in 1 ... scale - 1 {
        mainWindow.write(" ")
    }
        mainWindow.cursor.position = Point(x:X1, y:Y3)
        mainWindow.write("┼")
        for _ in 1 ... scale {
        mainWindow.write("─")
    }
        X1 = X1 + scale

        i = i + 1
        
    }

    mainWindow.cursor.position = Point(x:X1, y:Y1)
    mainWindow.write("┐")
    mainWindow.cursor.position = Point(x:X1, y:Y2)
    mainWindow.write("│")
    mainWindow.cursor.position = Point(x:X1, y:Y3)
    mainWindow.write("┤")

    
}

//Function to fill gaps in the boxes(suposedly every 6 spaces)
func linesTofillGaps(X:Int, Y:Int, Times:Int) {
    var X = X
    for _ in 0 ... Times {
        
        mainWindow.cursor.position = Point(x:X, y:Y)
        mainWindow.write("│")
        for _ in 1 ... scale - 1 {
        mainWindow.write(" ")
    }
        X = X + scale
    }
    
}

func BottomOfGrid(X:Int, Y:Int, Times:Int) {
    var X = X
    mainWindow.cursor.position = Point(x:X, y:Y)
    mainWindow.write("└")
    for _ in 1 ... scale - 1 {
        mainWindow.write("─")
    }
    X = X + scale
    for _ in 0 ... Times {
        mainWindow.cursor.position = Point(x:X, y:Y)
        mainWindow.write("┴")
        for _ in 1 ... scale - 1 {
        mainWindow.write("─")
    }
        
        X = X + scale
        
    }
    mainWindow.cursor.position = Point(x:X, y:Y)
    mainWindow.write("┘")
}

func threeByThree(X:Int, Y:Int) {
    
    
    TopLineBox(X1:X, Y1:Y, Times:2) 
    LineofBox(X1:X, Y1:Y + 2, Times:2)

    //LineofBox(X1:15, Y1:16, Times:2)

    linesTofillGaps(X:X, Y:Y +  6 - 1, Times:3) 

    BottomOfGrid(X:X, Y:Y + 6, Times:1) 

}

// Temporarily move the cursor to a new position
cursor.pushPosition(newPosition:Point(x:0, y:0))
mainWindow.write("Cursor is at: \(cursor.position)")
cursor.popPosition()

mainWindow.turnOff(redOnWhite)

mainWindow.refresh()

let keyboard = Keyboard.shared

while true {
    let key = keyboard.getKey(window:mainWindow)
    mainWindow.clearToEndOfLine()
    mainWindow.refresh()
    mainWindow.clear()

    for i in 0 ... 2 {
    for j in 0 ... 2 {
        threeByThree(X:size.width/2 + i * (scale*3+2), Y:10 + j * 7)
    }
}
    switch (key.keyType) {
    case .arrowDown: scale = scale - 1
        case .arrowUp: scale = scale + 1
                       default: scale = scale + 0 
    }
    if scale == 2 {
        scale = 3
    }
    
    
}
// Wait forever, or until the user presses CTRL-C
//screen.wait()
