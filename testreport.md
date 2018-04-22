
# **_TestReport_** 
## **_Test Option_**
> - Test the funtion `move()` of the class `Jumper`, the situation of the test will be  jump over a actor(include empty cell) and jump to a actor(include empty cell, too.<br>
> **_jump over the actor_**<br>
>> There will be five test function, include 
```
	//test Jumper can not jump over Rock
	@Test
	public void testJumpOverRock() {
	
	//test Jumper can not jump over Flower
	@Test
	public void testJumpOverFlower() {

	//test Jumper jump over Jumper
	@Test
	public void testJumpOverJumper() {

	//test Jumper jump over Bug
	@Test
	public void testJumpOverBug() {
	
	//test Jump over wall
	@Test
	public void jumpOverWall() {
```
> 
> **_jump to the actor_**
>> There will be six test function, include 
```
	//test Jumper can not jump to Rock
	@Test
	public void testJumpToRock() {
	
	//test Jumper can not jump to Flower
	@Test
	public void testJumpToFlower() {
	
	//test Jumper jump to Jumper
	@Test
	public void testJumpToJumper() {
	
	//test Jumper jump to Bug
	@Test
	public void testJumpToBug() {
	
	//test Jump to empty
	@Test
	public void jumpToEmpty() {
	
	//test Jump to wall
	@Test
	public void jumpToWall() {
```
-----
## **_function_**
> **_jump over_**
```
	//test Jumper can not jump over Rock
	@Test
	public void testJumpOverRock() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(2, 1));			//NOSONAR
		Location target = new Location (0, 1);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jumper can not jump over Flower
	@Test
	public void testJumpOverFlower() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(3, 2));			//NOSONAR
		Location target = new Location (1, 2);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jumper jump over Jumper
	@Test
	public void testJumpOverJumper() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(4, 3));			//NOSONAR
		Location target = new Location (4, 3);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}

	
	//test Jumper jump over Bug
	@Test
	public void testJumpOverBug() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(3, 2));			//NOSONAR
		Location target = new Location (1, 2);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jump over wall
	@Test
	public void jumpOverWall() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(0, 6));			//NOSONAR
		Location firstTarget = new Location (0, 6);	//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(firstTarget));
		jump.act();
		jump.act();
		Location secondTarget = new Location(0, 8);	//NOSONAR
		assertTrue(jump.getLocation().equals(secondTarget));
	}
```
> **_jump to actor_**
```
	//test Jumper can not jump to Rock
	@Test
	public void testJumpToRock() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(3, 1));			//NOSONAR
		Location target = new Location (3, 1);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jumper can not jump to Flower
	@Test
	public void testJumpToFlower() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(4, 2));			//NOSONAR
		Location target = new Location (4, 2);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jumper jump to Jumper
	@Test
	public void testJumpToJumper() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(6, 4));			//NOSONAR
		Location target = new Location (6, 4);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}

	
	//test Jumper jump to Bug
	@Test
	public void testJumpToBug() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(5, 3));			//NOSONAR
		Location target = new Location (5, 3);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jump to empty
	@Test
	public void jumpToEmpty() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(5, 5));			//NOSONAR
		Location target = new Location (3, 5);		//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(target));
	}
	
	//test Jump to wall
	@Test
	public void jumpToWall() {
		jump.setDirection(0);						//NOSONAR
		jump.moveTo(new Location(1, 6));			//NOSONAR
		Location firstTarget = new Location (1, 6);	//NOSONAR
		jump.act();
		assertTrue(jump.getLocation().equals(firstTarget));
		jump.act();
		jump.act();
		Location secondTarget = new Location(1, 8);	//NOSONAR
		assertTrue(jump.getLocation().equals(secondTarget));
	}
```
