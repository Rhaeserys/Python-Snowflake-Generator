"""
Python-Snowflake-Generator

Using Python to create a snowflake.
To run this code properly you will need to save the file as .py in a notebook file if you are running Windows based PC.
Right click on the file where you have saved it and choose run with Python if you already have it installed.

Koch.py

A program to draw a Koch Snowflake using Python's turtle graphics.
This script should be run from a terminal, not a Jupyter Notebook.
"""


import turtle
import math

def drawKochSF(x1, y1, x2, y2, t):
    """
    Recursively draws a segment of the Koch snowflake.
    
    Args:
        x1, y1: Coordinates of the starting point of the line segment.
        x2, y2: Coordinates of the ending point of the line segment.
        t: The turtle object.
    """
    # Calculate the distance of the segment
    d = math.sqrt((x1 - x2)**2 + (y1 - y2)**2)
    
    # If the segment is too small, just draw a straight line
    if d < 10:
        t.up()
        t.goto(x1, y1)
        t.down()
        t.goto(x2, y2)
        return

    # --- Calculate the 3 points that will form the peak ---
    # Point p1 is 1/3 of the way along the line
    p1x = x1 + (x2 - x1) / 3.0
    p1y = y1 + (y2 - y1) / 3.0

    # Point p3 is 2/3 of the way along the line
    p3x = x1 + (x2 - x1) * 2.0 / 3.0
    p3y = y1 + (y2 - y1) * 2.0 / 3.0

    # Point p2 is the peak of the equilateral triangle
    # This involves a 60-degree rotation of the segment from p1
    angle = math.atan2(y2 - y1, x2 - x1) # Angle of the base segment
    p2x = p1x + (p3x - p1x) * math.cos(math.radians(60)) - (p3y - p1y) * math.sin(math.radians(60))
    p2y = p1y + (p3x - p1x) * math.sin(math.radians(60)) + (p3y - p1y) * math.cos(math.radians(60))

    # --- Recursive calls for each of the 4 new, smaller segments ---
    drawKochSF(x1, y1, p1x, p1y, t)
    drawKochSF(p1x, p1y, p2x, p2y, t)
    drawKochSF(p2x, p2y, p3x, p3y, t)
    drawKochSF(p3x, p3y, x2, y2, t)

def main():
    """Sets up the turtle and starts the drawing process."""
    print('Drawing the Koch Snowflake...')
    
    # Set up the screen and turtle
    screen = turtle.Screen()
    screen.bgcolor("black")
    
    t = turtle.Turtle()
    t.hideturtle()
    t.color("cyan")
    t.speed("fastest") # Use 0 for max speed
    t.pensize(2)

    # Define the 3 vertices of the initial equilateral triangle
    p1 = (-200, -100)
    p2 = (200, -100)
    p3_x = 0
    p3_y = -100 + 400 * math.sqrt(3) / 2.0 # Height of equilateral triangle
    p3 = (p3_x, p3_y)

    # Draw the three sides of the snowflake
    drawKochSF(p1[0], p1[1], p2[0], p2[1], t)
    drawKochSF(p2[0], p2[1], p3[0], p3[1], t)
    drawKochSF(p3[0], p3[1], p1[0], p1[1], t)

    print("Drawing complete. Click on the screen to exit.")
    # Wait for the user to click anywhere on the screen to exit
    screen.exitonclick()

# This ensures the main function is called when the script is run
if __name__ == '__main__':
    main()
