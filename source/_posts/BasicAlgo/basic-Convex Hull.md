---
title: Convex Hull
date: 2022-05-15 14:40:52
summary: 凸包
categories: Convex Hull
tags:
- Andrew's Monotone Chain
- Graham Scan
---
## 凸包

### Andrew's Monotone Chain

```java

```

### Graham Scan

> 选择y值最小的点


```java
public class ConvexHullGrahamScan {

	/**
	 * The comparator just compares the cross product of two vectors to see which one is on the
	 * left side and which is on the right side. Actual angles don't need to be calculated.
	 * 
	 * @param points
	 * @param ref
	 */
	private void sortByAngle(List<? extends Point> points, Point ref) {
		Collections.sort(points, (b, c) -> {
			/*
			 * the ref point should always be pushed to the beginning
			 */
			if (b == ref) return -1;
			if (c == ref) return 1;
			
			int ccw = GraphUtils.ccw(ref, b, c);
			
			if (ccw == 0) {
				/*
				 * Handle collinear points. We can just use the x coordinate and not 
				 * bother with the y since the ratio of y/x is going to be the same
				 */
				if (Float.compare(b.x, c.x) == 0) {
					/*
					 * rare case of floats matching up in a vertical line, we want 
					 * the closer point to be first
					 */
					return b.y < c.y ? -1 : 1;				
				} else {
					return b.x < c.x ? -1 : 1;				
				}				
			} else {
				return ccw * -1;
			}
		});			
	}
	
	/**
	 * The main algorithm. 
	 * 
	 * @param points
	 * @return
	 */
	public List<Point> scan(List<? extends Point> points) {
		Deque<Point> stack = new ArrayDeque<Point>();
		
		/*
		 * bottom most, left most point is guaranteed to be on the hull
		 */
		Point minYPoint = GraphUtils.getMinY(points);		
		sortByAngle(points, minYPoint); // sort by angle with respect to minYPoint
		
		stack.push(points.get(0)); // 1st point is guaranteed to be on the hull
		stack.push(points.get(1)); // don't know about this one yet
		
		for (int i = 2, size = points.size(); i < size; i++) {
			Point next = points.get(i);
			Point p = stack.pop();			
			
			while (stack.peek() != null && GraphUtils.ccw(stack.peek(), p, next) <= 0) { 
				p = stack.pop(); // delete points that create clockwise turn
			}
						
			stack.push(p);
			stack.push(points.get(i));
		}
		
		/*
		 * the very last point pushed in could have been collinear, so we check for that
		 */
		Point p = stack.pop();
		if (GraphUtils.ccw(stack.peek(), p, minYPoint) > 0) {
			stack.push(p); // put it back, everything is fine
		}
		
		return new ArrayList<>(stack);
	}
	
	public static void main(String[] args) {
		List<Point> points = new ArrayList<>();
		
		points.add(new Point(2, 2));
		points.add(new Point(-2, 3));
		points.add(new Point(1, 1));		
		
		ConvexHullGrahamScan hull = new ConvexHullGrahamScan();
		System.out.println("Graham Scan:" + hull.scan(points));
	}
}
```


### Jarvis March

> 选择y值最小的点
> 
> 
> O(n * h) 

```java
public class ConvexHullJarvisMarch {
	public List<Point> marchByAngle(Collection<? extends Point> points) {
		List<Point> hull = new ArrayList<>();
		
		Point startingPoint = GraphUtils.getMinY(points); // starting point guaranteed to be on the hull		
		hull.add(startingPoint);
				
		Point prev = startingPoint;
		float prevAngle = -1;
		
		while (true) {
			float minAngle = Float.MAX_VALUE;
			double maxDist = 0;
			Point next = null;
		
			/*
			 * iterate over every point and pick the one that creates the largest angle
			 */
			for (Point p : points) {
				if (p == prev) continue;
				
				float angle = GraphUtils.angle(prev, p);
				double dist = GraphUtils.dist(prev, p);
				int compareAngles = Float.compare(angle, minAngle);
				
				if (compareAngles <= 0 && angle > prevAngle) {
					if (compareAngles < 0 || dist > maxDist) {
						/*
						 * found a better Point. It either has a smaller angle, or if it's collinear, then it's further way
						 */
						minAngle = angle;
						maxDist = dist;
						next = p;						
					}
				}
			}
			
			if (next == startingPoint) break; // came back to the starting point, so we are done
			
			hull.add(next);
			
			prevAngle =  GraphUtils.angle(prev, next);
			prev = next;			
		}
		
		return hull;
	}
	
	
	/**
	 * 
	 * @param points
	 * @return
	 */
	public List<Point> march(Collection<? extends Point> points) {
		List<Point> hull = new ArrayList<>();
		
		Point startingPoint = GraphUtils.getMinY(points); // bottom most, left most point is guaranteed to be on the hull		
		hull.add(startingPoint);
				
		Point prevVertex = startingPoint;
		
		while (true) {
			Point candidate = null;		
			/*
			 * iterate over every point and pick the one that creates the smallest counterclockwise angle
			 * in reference to the previous vertex
			 */
			for (Point point : points) {
				if (point == prevVertex) continue;
				
				if (candidate == null) {
					candidate = point;
					continue;
				}
				
				int ccw = GraphUtils.ccw(prevVertex, candidate, point);
				
				if (ccw == 0 && GraphUtils.dist(prevVertex, candidate) < GraphUtils.dist(prevVertex, point)) {
					candidate = point; // collinear tie is decided by the distance
				} else if (ccw < 0) {
					/*
					 * if made a clockwise turn, then found a better point that makes a smaller 
					 * counterclockwise angle in reference to the previous vertex
					 */
					candidate = point; 
				}				
			}
			
			if (candidate == startingPoint) break; // came back to the starting point, so we are done
			
			hull.add(candidate);
			prevVertex = candidate;			
		}
		
		return hull;
	}
	
	public static void main(String[] args) {
		List<Point> points = new ArrayList<>();
		
		points.add(new Point(2, 2));
		points.add(new Point(1, 3));
		points.add(new Point(1, 1));		
		points.add(new Point(0, 2));
		points.add(new Point(3, 1));		
		
		ConvexHullJarvisMarch hull = new ConvexHullJarvisMarch();
		
		
		System.out.println("Jarvis March:" + hull.march(points));
	}
}
```


