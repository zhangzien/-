## Part3：GridWorld Classes and Interfaces
### The Location Class
#### Set 3
Assume the following statements when answering the following questions.

>Location loc1 = new Location(4, 3);<br>
>Location loc2 = new Location(3, 4);
* How would you access the row value for loc1?
  call the methord getRow();
  ~~~java
  //@file:info.gridworld.grid.Location.java
  //@location:106 ~ 113
   /**
     * Gets the row coordinate.
     * @return the row of this location
     */
    public int getRow()
    {
        return row;
    }
  ~~~
* What is the value of b after the following statement is executed?<br>
 ` boolean b = loc1.equals(loc2);`  <br>
 b = false;
 ~~~java
//@file:info.gridworld.grid.Location.java
//@location:197~212
  /**
     * Indicates whether some other <code>Location</code> object is "equal to"
     * this one.
     * @param other the other location to test
     * @return <code>true</code> if <code>other</code> is a
     * <code>Location</code> with the same row and column as this location;
     * <code>false</code> otherwise
     */
    public boolean equals(Object other)
    {
        if (!(other instanceof Location))
            return false;

        Location otherLoc = (Location) other;
        return getRow() == otherLoc.getRow() && getCol() == otherLoc.getCol();
    }
~~~



* What is the value of loc3 after the following statement is executed?<br>
`Location loc3 = loc2.getAdjacentLocation(Location.SOUTH);`<br>
(4,4)
 ~~~java
//@file:info.gridworld.grid.Location.java
//@location:124~130
 /**
     * Gets the adjacent location in any one of the eight compass directions.
     * @param direction the direction in which to find a neighbor location
     * @return the adjacent location in the direction that is closest to
     * <tt>direction</tt>
     */
    public Location getAdjacentLocation(int direction);
~~~

* What is the value of dir after the following statement is executed?<br>
`int dir = loc1.getDirectionToward(new Location(6, 5);`<br>
135(Southeast)
 ~~~java
//@file:info.gridworld.grid.Location.java
//@location:124~130
   /**
     * Returns the direction from this location toward another location. The
     * direction is rounded to the nearest compass direction.
     * @param target a location that is different from this location
     * @return the closest compass direction from this location toward
     * <code>target</code>
     */
    public int getDirectionToward(Location target)
    {
        int dx = target.getCol() - getCol();
        int dy = target.getRow() - getRow();
        // y axis points opposite to mathematical orientation
        int angle = (int) Math.toDegrees(Math.atan2(-dy, dx));

        // mathematical angle is counterclockwise from x-axis,
        // compass angle is clockwise from y-axis
        int compassAngle = RIGHT - angle;
        // prepare for truncating division by 45 degrees
        compassAngle += HALF_RIGHT / 2;
        // wrap negative angles
        if (compassAngle < 0)
            compassAngle += FULL_CIRCLE;
        // round to nearest multiple of 45
        return (compassAngle / HALF_RIGHT) * HALF_RIGHT;
    }
~~~
* How does the getAdjacentLocation method know which adjacent location to return?<br>
  act according to the param direction,which means the operation of the params of dc and dr. At last  
  >  return new Location(getRow() + dr, getCol() + dc);
   ~~~java
  //@file:info.gridworld.grid.Location.java
  //@location:124~130
  /**
     * Gets the adjacent location in any one of the eight compass directions.
     * @param direction the direction in which to find a neighbor location
     * @return the adjacent location in the direction that is closest to
     * <tt>direction</tt>
     */
    public Location getAdjacentLocation(int direction)
    // reduce mod 360 and round to closest multiple of 45
        int adjustedDirection = (direction + HALF_RIGHT / 2) % FULL_CIRCLE;
        if (adjustedDirection < 0)
            adjustedDirection += FULL_CIRCLE;

        adjustedDirection = (adjustedDirection / HALF_RIGHT) * HALF_RIGHT;
        int dc = 0;
        int dr = 0;
        if (adjustedDirection == EAST)
            dc = 1;
        else if (adjustedDirection == SOUTHEAST)
        {
            dc = 1;
            dr = 1;
        }
        else if (adjustedDirection == SOUTH)
            dr = 1;
        else if (adjustedDirection == SOUTHWEST)
        {
            dc = -1;
            dr = 1;
        }
        else if (adjustedDirection == WEST)
            dc = -1;
        else if (adjustedDirection == NORTHWEST)
        {
            dc = -1;
            dr = -1;
        }
        else if (adjustedDirection == NORTH)
            dr = -1;
        else if (adjustedDirection == NORTHEAST)
        {
            dc = 1;
            dr = -1;
        }
        return new Location(getRow() + dr, getCol() + dc);
  ~~~
### The Grid Interface
#### Set 4
* How can you obtain a count of the objects in a grid? How can you obtain a count of the empty locations in a bounded grid?
  
  we can first call the list methord to get the List of of the objects in a grid.then call `ArrayList().size()` to get counts. getNumRows() * getNumCols() - `ArrayList().size()` to
  get a count of the empty locations in a bounded grid.
  ~~~java
  //@file:info.gridworld.grid.Grid.java
  //@location:81~85
  /**
     * Gets the locations in this grid that contain objects.
     * @return an array list of all occupied locations in this grid
     */
    ArrayList<Location> getOccupiedLocations();
  ~~~

* How can you check if location (10,10) is in a grid?<br>
  call the methord `isValid(Location loc)`
  ~~~java
   //@file:info.gridworld.grid.Grid.java
  //@location:43~50
  /**
     * Checks whether a location is valid in this grid. <br />
     * Precondition: <code>loc</code> is not <code>null</code>
     * @param loc the location to check
     * @return <code>true</code> if <code>loc</code> is valid in this grid,
     * <code>false</code> otherwise
     */
    boolean isValid(Location loc);
  ~~~
* Grid contains method declarations, but no code is supplied in the methods. Why? Where can you find the implementations of these methods?<br>
  It functions as a interface to declare the methords.Other classes implement different methods by calling this interface, which is slightly different from other classes.(不同类实现方法略有差别) The interface is a good way to implement the characteristics of compact structure(结构紧密性) and simplified code.<br>
  we can find the implementations of these methods in `Abstract.java`
  ~~~java
  //@file:info.gridworld.grid.Grid.java
  //@location:26
  public abstract class AbstractGrid<E> implements Grid<E>
  ~~~
* All methods that return multiple objects return them in an ArrayList. Do you think it would be a better design to return the objects in an array? Explain your answer.<br>
   No.Many times we dont`t konw the number of the objects.If we
   use an array,the space is not totally made use of or the number of objects will exceed the max size of the array.But the arraylist perfectly solve the two questions.
### The Actor Class
#### Set 5
* Name three properties of every actor.<br>
 location,direction,color
  ~~~java
  //@file:info.gridworld.actor.Actor.java
  //@location:   32~34
    private Location location;
    private int direction;
    private Color color;
  ~~~
* When an actor is constructed, what is its direction and color?
  north;blue
  ~~~java
   //@file:info.gridworld.actor.Actor.java
   //@location:   36~45
     /**
     * Constructs a blue actor that is facing north.
     */
    public Actor()
    {
        color = Color.BLUE;
        direction = Location.NORTH;
        grid = null;
        location = null;
    }
  ~~~

* Why do you think that the Actor class was created as a class instead of an interface?<br>
  Many methords don`t need to change.When we extends the Actor calss,we can directly use the methord.Even if we want to change the method, we just have to override this method and this method

* Can an actor put itself into a grid twice without first removing itself? Can an actor remove itself from a grid twice? Can an actor be placed into a grid, remove itself, and then put itself back? Try it out. What happens?<br>
  An actor can`t put itself into a grid twice without first removing itself.It will throw an error. An actor can not remove itself from a grid twice.An actor can be placed into a grid, remove itself, and then put itself back.
  ~~~java
   //@file:info.gridworld.actor.Actor.java
   //@location:   115~127
     public void putSelfInGrid(Grid<Actor> gr, Location loc)
    {
        if (grid != null)
            throw new IllegalStateException(
                    "This actor is already contained in a grid.");

        Actor actor = gr.get(loc);
        if (actor != null)
            actor.removeSelfFromGrid();
        gr.put(loc, this);
        grid = gr;
        location = loc;
    }
  ~~~
  ~~~java
   //@file:info.gridworld.actor.Actor.java
   //@location:   115~127
  public void removeSelfFromGrid()
    {
        if (grid == null)
            throw new IllegalStateException(
                    "This actor is not contained in a grid.");
        if (grid.get(location) != this)
            throw new IllegalStateException(
                    "The grid contains a different actor at location "
                            + location + ".");

        grid.remove(location);
        grid = null;
        location = null;
    }
  ~~~

* How can an actor turn 90 degrees to the right?
  `setDirection(getDirection() + Location.RIGHT);`
  ~~~java
   //@file:info.gridworld.actor.Actor.java
   //@location:   74~85
  /**
     * Sets the current direction of this actor.
     * @param newDirection the new direction. The direction of this actor is set
     * to the angle between 0 and 359 degrees that is equivalent to
     * <code>newDirection</code>.
     */
    public void setDirection(int newDirection)
    {
        direction = newDirection % Location.FULL_CIRCLE;
        if (direction < 0)
            direction += Location.FULL_CIRCLE;
    }
  ~~~
### Extending the Actor Class
#### Set 6
* Which statement(s) in the canMove method ensures that a bug does not try to move out of its grid?
  ~~~java
  //@file:info.gridworld.actor.Bug.java
  //@location:96~99
  Location loc = getLocation();
  Location next = loc.getAdjacentLocation(getDirection());
  if (!gr.isValid(next))
     return false;
  ~~~
* Which statement(s) in the canMove method determines that a bug will not walk into a rock?
  ~~~java
  //@file:info.gridworld.actor.Bug.java
  //@location:100~101
    Actor neighbor = gr.get(next);
    return (neighbor == null) || (neighbor instanceof Flower);
  ~~~
* Which methods of the Grid interface are invoked by the canMove method and why?<br>
  `isValid` `get` 
  ~~~java
   //@file:info.gridworld.actor.Bug.java
  //@location:98~99
  if (!gr.isValid(next))
    return false;
  // @location: 100
  Actor neighbor = gr.get(next);
  ~~~
  
  ~~~java
  //@file:info.gridworld.grid.Grid.java
  //@location:50
  boolean isValid(Location loc);

  //@location: 98~99
  if (!gr.isValid(next))
    return false;
  ~~~
* Which method of the Location class is invoked by the canMove method and why?<br>
  `getAdjacentLocation`
   ~~~java
   //@file:info.gridworld.actor.Bug.java
  //@location:97
  Location next = loc.getAdjacentLocation(getDgetDirection());
   ~~~
  ~~~java
  //@file:info.gridworld.grid.Location.java
  //@location: 130
  public Location getAdjacentLocation(int direction)
  ~~~
* Which methods inherited from the Actor class are invoked in the canMove method?<br>
  `getGrid`  `getLocation`
  ~~~java
   //@file:info.gridworld.actor.Bug.java
  //@location:73,76
  Grid<Actor> gr = getGrid();
  Location loc = getLocation();
   ~~~ 
   ~~~java
    //@file:info.gridworld.actor.Actor.java
   //@location:   92~94  102~105
   public Grid<Actor> getGrid()
    {
        return grid;
    }

     public Location getLocation()
    {
        return location;
    }
   ~~~
* What happens in the move method when the location immediately in front of the bug is out of the grid?<br>
  call the methord`removeSelfFromGrid()`to remove itself.
  ~~~java
  //@file:info.gridworld.actor.Bug.java
  //@location:78~81
  if (gr.isValid(next))
          moveTo(next);
   else
          removeSelfFromGrid();
  ~~~
* Is the variable loc needed in the move method, or could it be avoided by calling getLocation() multiple times?<br>
  Yes,A lots of times,the location is  changed when we run the bug.we judging whether the bug can move firstly have to konw the bug's current position.<br>
  We can create a private member `Location`to avoid calling getLocation() multiple times.
* Why do you think the flowers that are dropped by a bug have the same color as the bug?<br>
  the actor is the bug.So we call `getColor()` to get the bug's color.
   ~~~java
   //@file:info.gridworld.actor.Bug.java
  //@location:82
   Flower flower = new Flower(getColor());
   ~~~
   ~~~java
   //@file:info.gridworld.actor.Actor.java
   //@location:   47~54
   /**
     * Gets the color of this actor.
     * @return the color of this actor
     */
    public Color getColor()
    {
        return color;
    }
   ~~~
* When a bug removes itself from the grid, will it place a flower into its previous location?<br>
  No,a bug calls the methord to remove itself.But the methord does nothing about flower.
   ~~~java
   //@file:info.gridworld.actor.Actor.java
   //@location:   133~146
   /**
     * Gets the color of this actor.
     * @return the color of this actor
     */
      public void removeSelfFromGrid()
    {
        if (grid == null)
            throw new IllegalStateException(
                    "This actor is not contained in a grid.");
        if (grid.get(location) != this)
            throw new IllegalStateException(
                    "The grid contains a different actor at location "
                            + location + ".");

        grid.remove(location);
        grid = null;
        location = null;
    }
   ~~~
* Which statement(s) in the move method places the flower into the grid at the bug’s previous location?<br>
   ~~~java
   //@file:info.gridworld.actor.Bug.java
  //@location:82
   Flower flower = new Flower(getColor());
  flower.putSelfInGrid(gr, loc);
   ~~~
* If a bug needs to turn 180 degrees, how many times should it call the turn method?<br>
  a `turn()` turns 45 degrees,so 4 times.