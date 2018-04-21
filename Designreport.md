# <center>Design Report</center>
## **_Clarify the detail_**
> What will a jumper do if the location in front of it is empty, but the location two cells in front contains a flower or a rock?<br>
> - Answer: The Jumper can not jump forward, so it will turn until it can jump, and then it will jump.<br>
> 
> What will a jumper do if the location two cells in front of the jumper is out of the grid?<br>
> - Answer: The Jumper can not jump forward, so it will turn until it can jump (actually, it will turn at least 90 degrees) and jump.<br>
>  
>  What will a jumper do if it is facing an edge of the grid?<br>
>  - Answer: The Jumper can not jump, so it will turn until it can jump (at least 90 degrees) and jump.<br>
>   
>   What will a jumper do if another actor (not a flower or a rock) is in the cell that is two cells in front of the jumper?<br>
>  Answer: The Jumper can not jump at the cell which almost have other actor. So it will turn until it can jump and jump.<br>
>   
>   What will a jumper do if it encounters another jumper in its path?<br>
>   -Answer: It can not jump over another Jumper, actually, Jumper just can jump over flower or rock, of course, and empty cell.<br>
>    
>    Are there any other tests the jumper needs to make?<br>
>    - Answer: Maybe what will happen if a jumper was around by other jumpers, and maybe what will happen if a jumper behind another jumper and they have same direction.<br>
> - And in my design, for the first question, the jumper will turn and turn until other jumper jump away. For the second question, the solution is that which jumper will jump first, if the front jumper jump first, when the second start to jump, the cell behind it is empty (jumper has jump away). Otherwise, the back jumper will turn and jump.<br>
>  
------
## **_The design_**
> My `Jumper` will extends the class `Actor`. Actually , it will very similar as the class `Bug`, the different between them is that `Jumper` needs to charge two cells in front of it, but `Bug` only needs one. So the different between them will in the function `move()` and `canMove()`.<br>
So the construct of Jumper will be this:
```
public class Jumper extends Actor
{
    /**
     * Constructs a red jumper.
     */
    public Jumper()

    /**
     * Constructs a jumper of a given color.
     * @param jumperColor the color for this jumper
     */
    public Jumper(Color jumperColor)

    /**
     * Moves if it can move, turns otherwise.
     */
    public void act()

    /**
     * Turns the jumper 45 degrees to the right without changing its location.
     */
    public void turn()

    /**
     * Moves the jumper forward, putting nothing into the location it previously
     * occupied.
     */
    public void move()

    /**
     * Tests whether this Jumper can move forward into a location that is empty
     * Jumper can jump over the flower and rock, but cann't jump over the bug
     * @return true if this Jumper can move.
     */
    public boolean canMove()
}

```
-------
## **_The Construct_**
> This part is construct a `Jumper` class.<br>
> According to the design, the function `act()` `turn()` will as same as `Bug`. So we just pay attention to function `move()` and `canMove()`.

> - **`move()`**
```
	/**
     * Moves the jumper forward, putting nothing into the location it previously
     * occupied.
     */
    public void move()
    {
        Grid<Actor> gr = getGrid();
        if (gr == null)
            return;
        Location loc = getLocation();
        Location next = loc.getAdjacentLocation(getDirection());
        Location target = next.getAdjacentLocation(getDirection());
        if (gr.isValid(target))
            moveTo(target);
        else
            removeSelfFromGrid();			//remove the actor at the target location
    }
```
> 
> - **`canMove()`**
```
/**
     * Tests whether this Jumper can move forward into a location that is empty
     * Jumper can jump over the flower and rock, but cann't jump over the bug
     * @return true if this Jumper can move.
     */
    public boolean canMove()
    {
        Grid<Actor> gr = getGrid();
        if (gr == null)
            return false;
        Location loc = getLocation();
        Location next = loc.getAdjacentLocation(getDirection());
        Location target = next.getAdjacentLocation(getDirection());
        if (!gr.isValid(target))
            return false;
        Actor firstNeighbor = gr.get(next);			//first cell in front of Jumper
        Actor secondNeighbor = gr.get(target);		//second(target) cell in front of Jumper
        if (firstNeighbor instanceof Jumper) {		//can not jumper over a Jumper
        	return false;
        } else {
        	return (secondNeighbor == null);		//just can jump to an empty cell
        }
        // ok to move into empty location
        // not ok to move onto any other actor
    }
```
