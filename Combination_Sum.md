39.Combination Sum

[https://leetcode.com/problems/combination-sum/](https://leetcode.com/problems/combination-sum/)

Given a set of candidate numbers (C) (without duplicates) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [2, 3, 6, 7] and target 7, 
A solution set is: 
```shell
[
  [7],
  [2, 2, 3]
]
```

经典的递归回溯题目。

这题要注意几个点：

1.如果K比较大的话，我们可以适当优化，也就是说index的终止条件可以设置为n-k+1处，因为从那里开始
就不再能找到k个数字了嘛。然后每一次递归后k--，也就是指明我们还需要寻找的数字。

2. 跟排列题目的差别是：排列题目你可以在当前可能可以选的所有的数字中选，而combin只能往后选。
例子：
1, 2, 3, 4       k = 2
如果是排列的话： 我如果先选1, 2    1, 3    1, 4 然后 我们可以选2,1    2,3   2,4
但组合就不一样，1,2   1,3   1,4  然后到2只能选 2，3    2，4   因为2，1是重复的组合。
避免重复的办法就是，每一次只选当前数字之后的，不返回去选。这样保证了我们的组合是升序，也就是唯一序啦。

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {

		List<List<Integer>> result = new ArrayList<List<Integer>>();

		if (candidates == null || candidates.length == 0)
			return result;

		ArrayList<Integer> current = new ArrayList<Integer>();

		Arrays.sort(candidates);

		/**
		 * depth-first search(DFS).
		 * To solve DFS problem, recursion is a normal implementation.
		 * Note that the candidates array is not sorted, we need to sort it
		 * first.
		 */
		combinationSum(candidates, target, 0, current, result);

		return result;
	}

	/**
	 * 
	 * @param candidates 候选元素
	 * @param target 目标和
	 * @param j index
	 * @param curr 当前集合
	 * @param result 结果集合
	 */
	public void combinationSum(int[] candidates, int target, int j, ArrayList<Integer> curr, List<List<Integer>> result) {
		if (target == 0) {
			ArrayList<Integer> temp = new ArrayList<Integer>(curr);
			result.add(temp);
			return;
		}
		for (int i = j; i < candidates.length; i++) {
			if (target < candidates[i])
				return;
			curr.add(candidates[i]);
			combinationSum(candidates, target - candidates[i], i, curr, result);
			curr.remove(curr.size() - 1);
		}
	}
```
