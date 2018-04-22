# **_<center>Grid</center>_**
## **_SparseBoundedGrid_**
### 1.Implement
> - `SparseGridNode`
> - `SparseBoundedGrid<E> extends AbstractGrid<E>`
> - Class `SparseBoundedGrid` implement the data memory use a list which include many FrontNode, which linked all nodes of rows which the FrontNode locate. <br>
>  
> > - `SparseGridNode`
```
public class SparseGridNode
{
    private Object obj;
    private int col;
    private SparseGridNode next;
    
    public SparseGridNode(Object object, int column, SparseGridNode nextNode);
     
    public Object getObject();

    public int getColumn();

    public SparseGridNode getNextNode();

    public void setObject(Object object);

    public void setNext(SparseGridNode nextNode);
}
```
>> - `SparseBoundedGrid`
```
public class SparseBoundedGrid<E> extends AbstractGrid<E>  {
	private SparseGridNode[] objArray;
	private int col;
	private int row;
	/**
	* Constructs an empty bounded grid with the given dimensions.
	* (Precondition: <code>rows > 0</code> and <code>cols > 0</code>.)
	* @param rows number of rows in BoundedGrid
	* @param cols number of columns in BoundedGrid
	*/
	public SparseBoundedGrid(int tempRow, int tempCol);
	
	public int getNumRows();
	
	public int getNumCols();
	
	/**
     * Checks whether a location is valid in this grid. <br />
     * Precondition: <code>loc</code> is not <code>null</code>
     * @param loc the location to check
     * @return <code>true</code> if <code>loc</code> is valid in this grid,
     * <code>false</code> otherwise
     */
	public boolean isValid(Location loc);
	
	/**
     * Gets the locations in this grid that contain objects.
     * @return an array list of all occupied locations in this grid
     */
	public ArrayList<Location> getOccupiedLocations();
	
	//get the object of location
	public E get(Location loc);
	
	//remove the object of move location
	public E remove(Location moveLoc);
}
```
###  2.Important function
>
>> 1.**_`remove()`_**
```
	//remove the object of move location
	public E remove(Location moveLoc) {
		if (!isValid(moveLoc)) {
			throw new IllegalArgumentException("Location " + moveLoc + " is not valid");
		}
		E obj = get(moveLoc);
		if (obj == null) {
			return null;
		}
		//ptr of row
		SparseGridNode p = objArray[moveLoc.getRow()];
		if(p != null) {
			if(p.getColumn() == moveLoc.getCol()) {
				objArray[moveLoc.getRow()] = p.getNextNode();
			} else {
				SparseGridNode q = p.getNextNode();
				while(q!= null && q.getColumn() != moveLoc.getCol()) {
					p = p.getNextNode();
					q = q.getNextNode();
				}
				if(q != null) {
					p.setNext(q.getNextNode());
				}
			}
		}
		return obj;
	}
```
>> 2.**_`put()`_**
```
	//put object at grid
	public E put(Location loc, E obj) {
		if (!isValid(loc)) {
			throw new IllegalArgumentException("Location " + loc + " is not valid");
			
		}
		if (obj == null) {
			throw new NullPointerException("obj == null");
		}
		E oldObj = remove(loc);								//remove the Actor of location
		SparseGridNode p = objArray[loc.getRow()];
		objArray[loc.getRow()] = new SparseGridNode(obj, loc.getCol(), p);
		return oldObj;
	}
```
>> 3.**_`get()`_**
```
	//get the object of location
	public E get(Location loc) {
		if (!isValid(loc)) {
			throw new IllegalArgumentException("Location " + loc + " is not valid");
		}
		SparseGridNode p = objArray[loc.getRow()];
		while(p != null) {
			if(loc.getCol() == p.getColumn()) {
				return (E)p.getObject();
			} else {
				p = p.getNextNode();
			}
		}
		return null;
	}
```
>> 4.**_`getOccupiedLocations()`_**
```
	public ArrayList<Location> getOccupiedLocations() {
		ArrayList<Location> objLocation = new ArrayList<Location>();
		for (int i = 0; i < getNumRows(); i++) {
			SparseGridNode p = objArray[i];
			while(p != null) {
				Location loc = new Location(i, p.getColumn());
				objLocation.add(loc);
				p = p.getNextNode();
			}
		}
		return objLocation;
	}
```
## **_SparseBoundedGrid3_**
### 1.Implement
>> - Using HashMap to save data
>> - Just save the Object and its Location
>> - `SparseBoundedGrid3<E> extends AbstractGrid<E>`
```
public class SparseBoundedGrid3<E> extends AbstractGrid<E> {
	private HashMap<Location, E> objMap;		//NOSONAR
	private int row;
	private int col;
	/**
	* Constructs an empty unbounded grid.
	*/
	public SparseBoundedGrid3(int rows, int cols);
	public int getNumRows();
	@Override
	public int getNumCols();
	@Override
	public boolean isValid(Location loc);
	@Override
	public E put(Location loc, E obj);
	@Override
	public E remove(Location loc);
	@Override
	public E get(Location loc);
	@Override
	public ArrayList<Location> getOccupiedLocations();
}
```
### 2.Important function
>> - 1.**_`put()`_**
```
	@Override
	public E put(Location loc, E obj) {
		if (loc == null) {
			throw new NullPointerException("location is null");	//NOSONAR
		}
		if (obj == null) {
			throw new NullPointerException("object is null");	//NOSONAR
		}
		return objMap.put(loc, obj); 
	}
```
>> - 2.**_`remove()`_**
```
	public E remove(Location loc) {
		if (loc == null) {
			throw new NullPointerException("location is null");	//NOSONAR
		}
		return objMap.remove(loc); 
	}
```
>> - 3.**_`get()`_**
```
	public E get(Location loc) {
		if (loc == null) {
			throw new NullPointerException("location is null");	//NOSONAR
		}
		return objMap.get(loc); 
	}
```
>> - 4.**_`getOccupiedLocations()`_**
```
	public ArrayList<Location> getOccupiedLocations() {
		ArrayList<Location> occLocation = new ArrayList<Location>();	
		for (Location testLoc : objMap.keySet()) {
		    occLocation.add(testLoc);
                }
		return occLocation; 
	} 
```
## **_UnboundedGrid2_**
### 1.Implement
>> - Two-dimensional array of Object to implement the data save.
>> - Initial the Grid of size 8, use function `extends()` to extends the size of Grid.
>> - `UnboundedGrid2<E> extends AbstractGrid<E>`
### 2.Important function
>> - 1.**_`extend()`_**
```
	//extends the map size as twice as before
	public void extend () {
		Object[][] tempMap = new Object[size][size];
		for (int i = size - 1; i > 0; i--) {			//NOSONAR
			for (int j = 0; j < size; j++) {		//NOSONAR
				tempMap[i][j] = map[i][j];
			}
			map[i] = null;
		}
		map = null;
		int tempSize = size;
		size *= 2;						//NOSONAR
		map = new Object[size][size];
		for (int i = 0; i < tempSize; i++) {			//NOSONAR
			for (int j = tempSize - 1; j > 0; j--) {	//NOSONAR
				map[i][j] = tempMap[i][j];
			}
		}
	}
```
>> - 2.**_`put()`_**
```
	public E put(Location loc, E obj) {
		if (loc == null) {
			throw new NullPointerException("location is null"); 	
		}
		if (obj == null) {
			throw new NullPointerException("object is null");
		}
		while (loc.getRow() >= size || loc.getCol() >= size) {
			extend();
		}
		E oldOccupant = get(loc);
		map[loc.getRow()][loc.getCol()] = obj;
		return oldOccupant; 
	}
```
>> - 3.**_`remove()`_**
```
	public E remove(Location loc) {
		if (!isValid(loc)) {
			throw new IllegalArgumentException("Location " + loc + " is not valid"); 
		}
		if(loc.getRow() >= size || loc.getCol() >= size) {
			return null;
		}
		E r = get(loc);
		map[loc.getRow()][loc.getCol()] = null;
		return r;
	}
```
>> - 4.**_`get()`_**
```
	public E get(Location loc) {
		if (!isValid(loc)) {
			throw new IllegalArgumentException("Location " + loc + " is not valid");
		}
		if(loc.getRow() >= size || loc.getCol() >= size) {
			return null;
		}
		return (E) map[loc.getRow()][loc.getCol()]; 
	}
```
>> - 5.**_`getOccupiedLocations()`_**
```
	public ArrayList<Location> getOccupiedLocations() {			//NOSONAR
		ArrayList<Location> occLocation = new ArrayList<Location>();
		for (int i = 0; i < size; i++) {				//NOSONAR
			for (int j = 0; j < size; j++) {			//NOSONAR
				Location testLoc = new Location(i, j);
				if (get(testLoc) != null) {
					occLocation.add(testLoc);
				}
			}
		}
		return occLocation;
	}
```
