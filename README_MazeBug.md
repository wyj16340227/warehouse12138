# <center>**_MazeBug_**</center>
## **_Implement_**<br>
> 
- Rewrite the function `act()`, `getValid()`,  `canMove()`, `move()`, and write my function `goBack()`, `directionPredic()`. <br>
- Implement the predict as the induction from `github`, and then, predict the first step of the bug, use the function `sirectionPredic()`, to charge where the end rock, and add the weight(set it more than others).<br>
- When the `Bug` get the `red rock`, it will stop, and change the `flower` color to `green` which at the best path. <br>
- The bug get the valid cell Location and charge the best direction to move.<br>
- Use the `Stack<ArrayList>`, the element of stack save Location and will reach Location, except the tail element, tail element save the only one element, current Location.<br>

## **_Key Code_**
### **_directionPredict()_**
> - Implement<br>
>  
>> Get the Occupied Location, and charge the Location of Bug and red rock, to set the value of predict array.(Higher the direction of red rock).<br>
>
```
    public void directionPredic(){
        Grid<Actor> grid = getGrid();
        ArrayList<Location> array = grid.getOccupiedLocations();

        for( Location a : array ){
            Actor act = (Actor) grid.get(a);  
            if ( act instanceof Rock && act.getColor().equals(Color.RED)){  
                Location loc = this.getLocation();  
                if( loc.getRow() < a.getRow() ){  
                	predictDir[2] = 6;  
                	predictDir[0] = 1;  
                }else {  
                	predictDir[0] = 6;  
                	predictDir[2] = 1;  
                }  
                if( loc.getCol() < a.getCol() ){  
                	predictDir[1] = 6;  
                	predictDir[3] = 1;  
                }else {  
                	predictDir[3] = 6;  
                	predictDir[1] = 1;  
                }  
                break;  
            }  
        }  
    }  
```

### **_getValid()_**
> - Implement<br>
>  
>> Get the four Location around the bug, and the Location must be empty, except red rock. (flower mean this Location has been visit, so can not reach it again) If get the red rock, return the array list with the only element(red rock) and set `isEnd` to `true`<br>
>
```
    public ArrayList<Location> getValid(Location loc) {  
        Grid<Actor> grid = getGrid();
        if (grid == null) {
            return null;
        }
        ArrayList<Location> valid = new ArrayList<Location>();
        //get the valid location of the determined four direction
        int[] dir = {Location.NORTH, Location.EAST, Location.SOUTH, Location.WEST};  
        for( int i = 0; i < 4; i++ ){  
            Location tempLoc = loc.getAdjacentLocation(dir[i]);
            //charge the location valid or not
            if(grid.isValid(tempLoc)){  
                Actor actor = grid.get(tempLoc);
                //win the game
                if((actor instanceof Rock) && actor.getColor().equals(Color.RED) ){  
                    next = tempLoc;  
                    ArrayList<Location> end = new ArrayList<Location>();  
                    end.add(next);
                    int facing = this.getLocation().getDirectionToward(next);
                    this.setDirection(facing);
                    isEnd = true;
                    return end;  
                }else if(actor == null){  
                    valid.add(tempLoc);  
                }  
            }  
        }  
        return valid;  
    }  
 ```
 
### **move()_**
> - Implement<br>
>  
>> First, get the direction which has most probability, if the Location has more than one, create a random value, to charge the direction. <br>
>> Second, move to the Location and facing to the direction.<br>
>> Third, push the location to the stack and right way.<br>
>> Finally, leave a flower.<br>
>
```
    public void move() {  
        Grid<Actor> grid = getGrid();  
        if (grid == null) {
            return;
        }
        Location loc = this.getLocation();
        ArrayList<Location> allLocation = getValid(loc);
        //根据当前每个方向走了多少次来分配概率。某一方向走的步数最多，则往该方向概率最大  
        int max = 0;  
        int currentDir = 0;  
        int totalDir = 0;  
        int moveLoc = 0;  
        for(Location tempLoc : allLocation) {
            int tempDir = loc.getDirectionToward(tempLoc);
            //more step in this direction
            if(predictDir[tempDir / 90] > max) {
                max = predictDir[tempDir / 90];
                moveLoc = totalDir;
                currentDir = (int)tempDir / 90;
            }  
            totalDir++;  
        }
          
        if(allLocation.size() == 1) {  
            next = allLocation.get(moveLoc);
            predictDir[currentDir]++;  
        } else {
        	//create a random number to decide which Location to go
            int tempNum = (int) (Math.random() * 10);  
            if(tempNum >= 0 && tempNum < 7){  
                next = allLocation.get(moveLoc);
                predictDir[currentDir]++;
            }else {  
                next = allLocation.get(tempNum % allLocation.size()); 
                currentDir = loc.getDirectionToward(next) / 90;  
                predictDir[currentDir]++;  
            }  
        }  
          
        for( Location tempLoc : allLocation ){
            if(this.getDirection() == loc.getDirectionToward(tempLoc)){
                next = tempLoc;
                int tempDir = loc.getDirectionToward(next);  
                currentDir = tempDir / 90;  
                predictDir[currentDir]++;  
                break;  
            }  
        }  
          
        if (grid.isValid(next)) {
            moveTo(next);  
            way.push(next);  
            int facing = loc.getDirectionToward(next);
            this.setDirection(facing);  
              
            ArrayList<Location> temp = crossLocation.pop();
            temp.add(next);  
            crossLocation.push(temp);
            ArrayList<Location> latest = new ArrayList<Location>();  
            latest.add(next);  
            crossLocation.push(latest);  
              
        } else {  
            removeSelfFromGrid();  
        }
        Flower flower = new Flower(getColor());  
        flower.putSelfInGrid(grid, loc);  
    }
```

### **goBack()_**
> - Implement<br>
>  
>> First, get the Location one step before now(the first element of the arrayList which at the second from tail of stack), and move to it.<br>
>> Then, decrease the value of the predict with last moving direction.<br>
>
```
    public void goBack(){  
        if (crossLocation.size() > 0) {  
            crossLocation.pop();  
            way.pop();  
            if(crossLocation.size() > 0){  
                Grid grid = getGrid();  
                if( grid == null ) {
                    return;
                }
                Location last = crossLocation.peek().get(0);
                Location currentLoc = this.getLocation();
                //set the direction when the bug return  
                int tempDir = currentLoc.getDirectionToward(last);  
                if (grid.isValid(last)) {  
                    this.setDirection(tempDir);  
                    moveTo(last);  
                    stepCount++;  
                }else {  
                    removeSelfFromGrid();
                }  
                if ((int)(tempDir/90) == 0) {  
                	predictDir[2]--;  
                }else if ((int)(tempDir/90) == 1) {  
                	predictDir[3]--;  
                }else if ((int)(tempDir/90) == 2) {  
                	predictDir[0]--;  
                }else if ((int)(tempDir/90) == 3) {  
                	predictDir[1]--;  
                }  
                Flower flower = new Flower(getColor());  
                flower.putSelfInGrid(grid, currentLoc);
            }  
        }  
    }  
```

### **_act()_**
> - Implement<br>
>  
>> If have not move, predict the direction of the end.<br>
>> If reach the red rock(end), charge has shown or not, if not, shown.<br>
>> If not reach the end, move.<br>
>
```
    public void act() {  
        //put start location to stack
        if(stepCount == 0){  
            Location local = this.getLocation();  
            directionPredic();
            ArrayList<Location> firstStep = new ArrayList<Location>();  
            firstStep.add(local);
            crossLocation.add(firstStep); 
            way.push(local);   
        }
        boolean willMove = canMove();  
        if (isEnd == true) {  
            showWay(way);  
        //show the step when game over
            if (hasShown == false) {  
                String msg = stepCount.toString() + " steps";  
                JOptionPane.showMessageDialog(null, msg);  
                hasShown = true;  
            }  
        } else if (!willMove) {  
            //If can't move, go back
            goBack();
        } else {
        	//if can move, move
            last = this.getLocation();
        	move();
            stepCount++;  
        }  
    }
```
