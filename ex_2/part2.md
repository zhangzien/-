~## Set 2 The source code for the BoxBug class can be found in the boxBug directory.
### 1. What is the role of the instance variable sideLength?
the max num of grid the bug can walk straight,when step equals sideLength,the bug will adjust to his direction.
~~~java
  // @file: GridWorldCode/projects/boxBug/BoXBug.java
  // @line: 43~57
  public void act()
    {
        if (steps < sideLength && canMove())
        {
            move();
            steps++;
        }
        else
        {
            turn();
            turn();
            steps = 0;
        }
    }
~~~
~~~java
    // @file: GridWorldCode/framework/info/gridworld/actor/Bug.java
    // @line: 59~65
    /**
     * Turns the bug 45 degrees to the right without changing its location.
     */
    public void turn()
    {
        setDirection(getDirection() + Location.HALF_RIGHT);
    }
~~~
### 2. What is the role of the instance variable steps?
To check whether the Boxbug can move straight(直走) in the next steps and know and documente the present steps of the BoxBug.
~~~java
  // @file: GridWorldCode/projects/boxBug/BoXBug.java
  // @line: 43~57
  public void act()
    {
        if (steps < sideLength && canMove())
        {
            move();
            steps++;
        }
        else
        {
            turn();
            turn();
            steps = 0;
        }
    }
~~~
### 3. Why is the turn method called twice when steps becomes equal to sideLength?
Each time wo call the "turn" methord,it turns the bug 45 degrees to the right,but we desire it to turn 90 degree(exactly turns to right),so it turns twice.
~~~java
    // @file: GridWorldCode/framework/info/gridworld/actor/Bug.java
    // @line: 59~65
    /**
     * Turns the bug 45 degrees to the right without changing its location.
     */
    public void turn()
    {
        setDirection(getDirection() + Location.HALF_RIGHT);
    }
~~~
### 4. Why can the move method be called in the BoxBug class when there is no move method in the BoxBug code?
It inherits the parent class Bug.java.Subclasses can call public methods of the parent class
~~~java
    // @file: GridWorldCode/framework/info/gridworld/actor/Bug.java
    // @line: 67~84
    /**
     * Moves the bug forward, putting a flower into the location it previously
     * occupied.
     */
    public void move()
    {
        Grid<Actor> gr = getGrid();
        if (gr == null)
            return;
        Location loc = getLocation();
        Location next = loc.getAdjacentLocation(getDirection());
        if (gr.isValid(next))
            moveTo(next);
        else
            removeSelfFromGrid();
        Flower flower = new Flower(getColor());
        flower.putSelfInGrid(gr, loc);
    }
~~~
### 5. After a BoxBug is constructed, will the size of its square pattern always be the same? Why or why not?
No,There is no other methords to change the sidelength except its original constructed methord.
~~~java
    // @file: GridWorldCode/framework/info/gridworld/actor/Bug.java
    // @line: 67~84
     /**
     * Constructs a box bug that traces a square of a given side length
     * @param length the side length
     */
    public BoxBug(int length)
    {
        steps = 0;
        sideLength = length;
    }
~~~
### 6. Can the path a BoxBug travels ever change? Why or why not?
Not always.For example,when the Boxbug moves to the edges of the grid,but steps is smaller than sidelength,it will change its direction,and the path changes.At the end, the route is fixed.And in the normal situation,the path does not change.
~~~java
    // @file: GridWorldCode/framework/info/gridworld/actor/Bug.java
    // @line: 67~84
    /**
     * Moves the bug forward, putting a flower into the location it previously
     * occupied.
     */
    public void move()
    {
        Grid<Actor> gr = getGrid();
        if (gr == null)
            return;
        Location loc = getLocation();
        Location next = loc.getAdjacentLocation(getDirection());
        if (gr.isValid(next))
            moveTo(next);
        else
            removeSelfFromGrid();
        Flower flower = new Flower(getColor());
        flower.putSelfInGrid(gr, loc);
    }
~~~

### 7. When will the value of steps be zero?
when steps becomes equal to the sideLength,after the methord "turn" is called twice,the steps will become zero.
~~~java
  // @file: GridWorldCode/projects/boxBug/BoXBug.java
  // @line: 43~57
  public void act()
    {
        if (steps < sideLength && canMove())
        {
            move();
            steps++;
        }
        else
        {
            turn();
            turn();
            steps = 0;
        }
    }
~~~
