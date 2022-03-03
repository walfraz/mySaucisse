# mySaucisse

from tkinter import*

window = Tk()
window.title("Saucisse")

POINT_RADIUS = 5

CANVA_WDT = 500
CANVA_HGT = 400

GRID_PARITY = 0

mainCanva = Canvas(window, width=CANVA_WDT, height=CANVA_HGT)
mainCanva.grid(row=0, column=0)

class Point:

    def __init__(self, x, y, marked=False, clicked=False, reality=True):
        self._coords = (x, y)
        self._marked = marked
        self._clicked = clicked
        self._reality = reality

    def getCoords(self):
        return self._coords

    def setCoords(self, x, y):
        self._coords = (x, y)

    def isMarked(self):
        return self._marked
    def setMarked(self):
        self._marked = True
    def setUnmarked(self):
        self._marked = False

    def setClicked(self):
        self._clicked = True
    def setUnclicked(self):
        self._clicked = False
    def isClicked(self):
        return self._clicked

    def setReality(self):
        self._reality = True
    def setVirtuality(self):
        self._reality = False
    def isVirtual(self):
        return self._virtuality

class Sausage:

    def __init__(self, x1, y1, x2, y2, x3, y3, color):
        self._body = ((x1, y1), (x2, y2), (x3, y3))
        self._color = color

    def getBody(self):
        return self._body

    def setColor(self, color):
        self._color = color

    def getColor(self):
        return self._color

class SausageFamilly:

    def __init__(self, color):
        self._color = color
        self._familly = []

    def addSausage(self, sausage):
        sausage.setColor(self._color)
        self._familly.append(sausage)

    def deleteSausage(self, sausage):
        for i in range(len(self._familly)):
            if sausage == self._familly[i]:
                self._familly.pop(i)

    def getColor(self):
        return self._color

    def setColor(self, color):
        self._color = color
        for sausage in self._familly:
            sausage.setColor(color)

class Grid:

    def __init__(self, width, height):
        self._size = (width, height)
        self._gridPoints = []
        for x in range(width):
            self._gridPoints.append([])
            for y in range(height):
                self._gridPoints[x].append(Point(x, y))

    def getSize(self):
        return self._size

    def getGridPoints(self):
        return self._gridPoints

class Screen:

    def __init__(self, canva):
        self._canva = canva
        self._widgets = []

    def drawGrid(self, grid, pointRadius, pointColor, parity=None):
        (gridWdt, gridHgt) = grid.getSize()
        padx = CANVA_WDT//(gridWdt+1)
        pady = CANVA_HGT//(gridHgt+1)
        for x in range(gridWdt):
            for y in range(gridHgt):
                if parity == None or (x+y)%2 == parity:
                    x1 = (x+1)*padx - pointRadius
                    y1 = (y+1)*padx - pointRadius
                    x2 = (x+1)*padx + pointRadius
                    y2 = (y+1)*padx + pointRadius
                    self._widgets.append(self._canva.create_oval(x1, y1, x2, y2, fill=pointColor, outline=pointColor))

    def drawSeg(self, x1, y1, x2, y2, size, color):
        self._widgets.append(self._canva.create_line(x1, y1, x2, y2, fill=color, capstyle="round", joinstyle="round", width=size))

class Controller:

    def __init__(self, screen):
        self._screen = screen

    def leftClick(self, evt):
        pass

grid = Grid(9, 7)
sausagesPlayer1 = SausageFamilly("blue")
sausagesPlayer2 = SausageFamilly("red")
screen = Screen(mainCanva)
screen.drawGrid(grid, 10, "blue", GRID_PARITY)
gameController = Controller(grid)

quitButton = Button(window,text='Quitter', command=window.destroy)
quitButton.grid(row=1, column=0)

mainCanva.bind('<Button-1>', gameController.leftClick)

window.mainloop()
