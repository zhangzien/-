# Part4：Interacting Objects
## The Critter Class
### Set 7
#### The source code for the Critter class is in the critters directory
* What methods are implemented in Critter?
  *Answer:* `act()`, `getActors()`,`processActors(ArrayList<Actor> actors)`,`getMoveLocations()`,` selectMoveLocation(ArrayList<Location> locs)`,`makeMove(Location loc)`.
  ~~~java
  //@file:info.gridworld.actor.Critter.java
  //@location:38
      public void act()
  //@location:56
     public ArrayList<Actor> getActors()
  //@location:71
     public void processActors(ArrayList<Actor> actors)
  //@location:88
    public ArrayList<Location> getMoveLocations()
  //@location:104
    public Location selectMoveLocation(ArrayList<Location> locs)
  //@location:125
     public void makeMove(Location loc)
  ~~~
* What are the five basic actions common to all critters when they act?
  *Answer:*`getGrid()`,`getActors()`,`getMoveLocations()`,`selectMoveLocation(moveLocs)`,`makeMove(loc)`
  ~~~java
  //@file:info.gridworld.actor.Critter.java
  //@location:38~47
    public void act()
    {
        if (getGrid() == null)
            return;
        ArrayList<Actor> actors = getActors();
        processActors(actors);
        ArrayList<Location> moveLocs = getMoveLocations();
        Location loc = selectMoveLocation(moveLocs);
        makeMove(loc);
    }
  ~~~
* Should subclasses of Critter override the getActors method? Explain.<br>
   *Answer:*By overriding the getActors methords,different subclasses of Critters can select thier own ways to select actors around to process.
* Describe the way that a critter could process actors.<br>
  `Answer:` an actor in the arraylist will be removed from the grid if it is not a rock or a Critter.
  ~~~java
  //@file:info.gridworld.actor.Critter.java
  //@location:61~78
  /**
     * Processes the elements of <code>actors</code>. New actors may be added
     * to empty locations. Implemented to "eat" (i.e. remove) selected actors
     * that are not rocks or critters. Override this method in subclasses to
     * process actors in a different way. <br />
     * Postcondition: (1) The state of all actors in the grid other than this
     * critter and the elements of <code>actors</code> is unchanged. (2) The
     * location of this critter is unchanged.
     * @param actors the actors to be processed
     */
   public void processActors(ArrayList<Actor> actors)
    {
        for (Actor a : actors)
        {
            if (!(a instanceof Rock) && !(a instanceof Critter))
                a.removeSelfFromGrid();
        }
    }
  ~~~
* What three methods must be invoked to make a critter move? Explain each of these methods.<br>
 *Answer:* ` getMoveLocations`  `selectMoveLocation` ` makeMove`<br>
 ` getMoveLocations`:Select a a list of possible locations which must be valid for the Critter's next move.
 ~~~java
 //@file:info.gridworld.actor.Critter.java
  //@location:80~91
 /**
     * Gets a list of possible locations for the next move. These locations must
     * be valid in the grid of this critter. Implemented to return the empty
     * neighboring locations. Override this method in subclasses to look
     * elsewhere for move locations.<br />
     * Postcondition: The state of all actors is unchanged.
     * @return a list of possible locations for the next move
     */
    public ArrayList<Location> getMoveLocations()
    {
        return getGrid().getEmptyAdjacentLocations(getLocation());
    }
 ~~~
`selectMoveLocation`:Select the location for the Critter's next move.
~~~java
//@file:info.gridworld.actor.Critter.java
  //@location:93~111
 /**
     * Selects the location for the next move. Implemented to randomly pick one
     * of the possible locations, or to return the current location if
     * <code>locs</code> has size 0. Override this method in subclasses that
     * have another mechanism for selecting the next move location. <br />
     * Postcondition: (1) The returned location is an element of
     * <code>locs</code>, this critter's current location, or
     * <code>null</code>. (2) The state of all actors is unchanged.
     * @param locs the possible locations for the next move
     * @return the location that was selected for the next move.
     */
    public Location selectMoveLocation(ArrayList<Location> locs)
    {
        int n = locs.size();
        if (n == 0)
            return getLocation();
        int r = (int) (Math.random() * n);
        return locs.get(r);
    }
~~~
` makeMove`:moves this critter to the given location.
~~~java
//@file:info.gridworld.actor.Critter.java
  //@location:113~131
 /**
     * Moves this critter to the given location <code>loc</code>, or removes
     * this critter from its grid if <code>loc</code> is <code>null</code>.
     * An actor may be added to the old location. If there is a different actor
     * at location <code>loc</code>, that actor is removed from the grid.
     * Override this method in subclasses that want to carry out other actions
     * (for example, turning this critter or adding an occupant in its previous
     * location). <br />
     * Postcondition: (1) <code>getLocation() == loc</code>. (2) The state of
     * all actors other than those at the old and new locations is unchanged.
     * @param loc the location to move to
     */
    public void makeMove(Location loc)
    {
        if (loc == null)
            removeSelfFromGrid();
        else
            moveTo(loc);
    }
~~~
* Why is there no Critter constructor?
  *Answer:*Since the Critter class inherits the Actor class, the default constructor of the parent class is called when the generated class has no constructor.
  ~~~java
  //@file:info.gridworld.actor.Critter.java
  //@location:113~131
   public class Critter extends Actor
  ~~~
## Extending the Critter Class
### Set 8
#### The source code for the ChameleonCritter class is in the critters directory
* Why does act cause a ChameleonCritter to act differently from a Critter even though ChameleonCritter does not override act?
  *Answer:* The methord act() calling the two methords are overrided in class ChameleonCritter.
     ~~~java
  //@file:projects.critters.ChamelenoCritter.java
  //@location:35,59
       public void processActors(ArrayList<Actor> actors)

       public void makeMove(Location loc)
  ~~~
* Why does the makeMove method of ChameleonCritter call super.makeMove?
  *Answer:*Simplyfy the code,the makeMove method of ChameleonCritter will use all contexts of the methord makeMove to move.And also,the overriding methord just changes the direction it moves,but the move action does not change.
  ~~~java
   //@file:projects.critters.ChamelenoCritter.java
  //@location:56~64
     /**
     * Turns towards the new location as it moves.
     */
    public void makeMove(Location loc)
    {
        setDirection(getLocation().getDirectionToward(loc));
        super.makeMove(loc);
    }

  ~~~

* How would you make the ChameleonCritter drop flowers in its old location when it moves?<br>
  *Answer:* override the makeMove methord again:
  ~~~java
    public void makeMove(Location loc)
    {
        Grid<Actor> gr = getGrid();
        setDirection(getLocation().getDirectionToward(loc));
        super.makeMove(loc);
        Flower flower = new Flower(getColor());
        flower.putSelfInGrid(gr, loc);
    }
  ~~~
* Why doesn’t ChameleonCritter override the getActors method?<br>
*Answer:* because the way it gets the actors is the same;

* Which class contains the getLocation method?
  *Answer:* the Actor class
  ~~~java
  //info.gridworld.actor.Actor.java
  //location:102~105
   public Location getLocation()
    {
        return location;
    }
  ~~~
* How can a Critter access its own grid?
  *Answer:* call the methord `getGrid()`.Critter inherits from Actor,So the class ChameleonCritter can call the public methords of the class Actor.
## Another Critter
### Set 9
#### The source code for the CrabCritter class is reproduced at the end of this part of GridWorld.

* Why doesn’t CrabCritter override the processActors method?<br>
  *Answer:* the way they processActors is the same:"eat" the actors that are not Rocks or Critters.So we don't have to override it.
  ~~~java
  //@file:info.gridworld.actor.Critter.java
  //@location:71~78

   public void processActors(ArrayList<Actor> actors)
    {
        for (Actor a : actors)
        {
            if (!(a instanceof Rock) && !(a instanceof Critter))
                a.removeSelfFromGrid();
        }
    }
  ~~~
* Describe the process a CrabCritter uses to find and eat other actors. Does it always eat all neighboring actors? Explain.<br>
  *Answer:*  By calling the methord `getActors()`,we can get the actors in front of ,left-front of,right-front of the Critters,and then calling the methords`processActor` to process a CrabCritter to eat actors.So they don't always eat all neighboring actors.
  ~~~java
  //@file:projects.critters.CrabCritter.java
  //@location:39~57
  /**
     * A crab gets the actors in the three locations immediately in front, to its
     * front-right and to its front-left
     * @return a list of actors occupying these locations
     */
    public ArrayList<Actor> getActors()
    {
        ArrayList<Actor> actors = new ArrayList<Actor>();
        int[] dirs =
            { Location.AHEAD, Location.HALF_LEFT, Location.HALF_RIGHT };
        for (Location loc : getLocationsInDirections(dirs))
        {
            Actor a = getGrid().get(loc);
            if (a != null)
                actors.add(a);
        }

        return actors;
    }
  ~~~

  ~~~java
    //@file:info.gridworld.actor.Critter.java
  //@location:71~78

   public void processActors(ArrayList<Actor> actors)
    {
        for (Actor a : actors)
        {
            if (!(a instanceof Rock) && !(a instanceof Critter))
                a.removeSelfFromGrid();
        }
    }
   ~~~
* Why is the getLocationsInDirections method used in CrabCritter?<br>
  *Answer:* to find the valid adjacent locations of this critter in different
  directions.
  ~~~java
    //@file:projects.critters.CrabCritter.java
  //@location:93~114
    /**
     * Finds the valid adjacent locations of this critter in different
     * directions.
     * @param directions - an array of directions (which are relative to the
     * current direction)
     * @return a set of valid locations that are neighbors of the current
     * location in the given directions
     */
    public ArrayList<Location> getLocationsInDirections(int[] directions)
    {
        ArrayList<Location> locs = new ArrayList<Location>();
        Grid gr = getGrid();
        Location loc = getLocation();
    
        for (int d : directions)
        {
            Location neighborLoc = loc.getAdjacentLocation(getDirection() + d);
            if (gr.isValid(neighborLoc))
                locs.add(neighborLoc);
        }
        return locs;
    }    
  ~~~

* If a CrabCritter has location (3, 4) and faces south, what are the possible locations for actors that are returned by a call to the getActors method?<br>
  *Answer:* (4,4)  (4,3) (4,5)
    ~~~java
  //@file:projects.critters.CrabCritter.java
  //@location:44~57
    public ArrayList<Actor> getActors()
    {
        ArrayList<Actor> actors = new ArrayList<Actor>();
        int[] dirs =
            { Location.AHEAD, Location.HALF_LEFT, Location.HALF_RIGHT };
        for (Location loc : getLocationsInDirections(dirs))
        {
            Actor a = getGrid().get(loc);
            if (a != null)
                actors.add(a);
        }

        return actors;
    }
  ~~~
* What are the similarities and differences between the movements of a CrabCritter and a Critter?<br>
  *Answer:*  similairities: Both of them move from the original place to 
the destinational palce;  To be frank,the regular is similar;And they both move to a valid place;<br>
  differences: 1) the critters can move the places that are valid and neighboring,but Crabcritters only can move to the place that is left or right 
  2)CrabCritters can turn 90 degree when they can move,but Critter can't.
 ~~~java
 //@file:projects.critters.CrabCritter.java
 //@location:74~91
   /**
     * If the crab critter doesn't move, it randomly turns left or right.
     */
    public void makeMove(Location loc)
    {
        if (loc.equals(getLocation()))
        {
            double r = Math.random();
            int angle;
            if (r < 0.5)
                angle = Location.LEFT;
            else
                angle = Location.RIGHT;
            setDirection(getDirection() + angle);
        }
        else
            super.makeMove(loc);
    }
 ~~~
  
  ~~~java
//@file:info.gridworld.actor.Critter.java
  //@location:125~131
    public void makeMove(Location loc)
    {
        if (loc == null)
            removeSelfFromGrid();
        else
            moveTo(loc);
    }
~~~


* How does a CrabCritter determine when it turns instead of moving?<br>
  *Answer:* if the param location is equal to the current location,it turns instead of moving. And when it turns determined by a random number(we call call it r). r is less than 0.5,it turns left.On the other hand,it turns right.
  ~~~java
  //@file:projects.critters.CrabCritter.java
  //@location:74~91
    /**
     * If the crab critter doesn't move, it randomly turns left or right.
     */
    public void makeMove(Location loc)
    {
        if (loc.equals(getLocation()))
        {
            double r = Math.random();
            int angle;
            if (r < 0.5)
                angle = Location.LEFT;
            else
                angle = Location.RIGHT;
            setDirection(getDirection() + angle);
        }
        else
            super.makeMove(loc);
    }
    
  ~~~
* Why don’t the CrabCritter objects eat each other?<br>
  *Answer:* See the methord `processActors`,they don't each other if they are Critters.And CrabCritter inherits from Critters, they are also Critters .
  So,they don't eat each other.<br>
  ~~~java
    //@file:info.gridworld.actor.Critter.java
  //@location:71~78

   public void processActors(ArrayList<Actor> actors)
    {
        for (Actor a : actors)
        {
            if (!(a instanceof Rock) && !(a instanceof Critter))
                a.removeSelfFromGrid();
        }
    }
  ~~~