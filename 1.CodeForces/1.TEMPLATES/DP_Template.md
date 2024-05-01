# My DP Templates

```
vector<vector<long long int>> dp(n+1,vector<long long int>(sum+1));
is Faster than 

```
## 1. Bounded Knapsack OR 0/1 Knapsace
Returns max Profit,we can make from certain elements(weight[] and value[])till weight of considered elements don't exceed Weight.
```
int boundedKnapsack01(vector<int> weight, vector<int> val, int Weight)
{
  int n = weight.size();
  int dp[n + 1][Weight + 1];
  for (int i = 0; i < n + 1; i++)
  {
    for (int j = 0; j < Weight + 1; j++)
    {
      if (i == 0 || j == 0)
      {
        dp[i][j] = 0;
      }
    }
  }
  for (int i = 1; i < n + 1; i++)
  {
    for (int j = 1; j < Weight + 1; j++)
    {
      if (weight[i - 1] <= j)
      {
        dp[i][j] = max(val[i - 1] + dp[i - 1][j - weight[i - 1]], dp[i - 1][j]);
      }
      else
      {
        dp[i][j] = dp[i - 1][j];
      }
    }
  }
  return dp[n][Weight];
}
```

## 2. SubSet Sum
Says whether Its possible to achieve a sum of target from array elements
```
bool subsetSum(vector<int> arr, int target)
{
  int n = arr.size();
  int dp[n + 1][target + 1];
  for (int i = 0; i < n + 1; i++)
  {
    for (int j = 0; j < target + 1; j++)
    {
      if (i == 0)
      {
        dp[i][j] = false;
      }
      if (j == 0)
      {
        dp[i][j] = true;
      }
    }
  }
  for (int i = 1; i < n + 1; i++)
  {
    for (int j = 1; j < target + 1; j++)
    {
      if (arr[i - 1] <= j)
      {
        dp[i][j] = dp[i - 1][j - arr[i - 1]] || dp[i - 1][j];
      }
      else
      {
        dp[i][j] = dp[i - 1][j];
      }
    }
  }
  return dp[n][target];
}
```

## 3. Equal Sum Partition
We need to tell If it is possible to distribute the array into exactly two parts i.e their sum is equal.
```
bool equalSumPartation(vector<int> arr)
{
  int totalSum = 0;
  for (int i = 0; i < arr.size(); i++)
  {
    totalSum += arr[i];
  }
  if (totalSum % 2 != 0) // Not possible if sum is odd
  {
    return false;
  }
  // Now see if it's possible to achieve the subset Sum(i.e totalSum/2)from array elements.
  return subsetSum(arr, totalSum / 2);
}
``` 

## 4. Target Sum/Perfect Sum Problem (working)
Count of subsets of having a particuler sum.
```
Input: arr[] = {1, 2, 3, 3}, X = 6 
Output: 3 
All the possible subsets are {1, 2, 3}, 
{1, 2, 3} and {3, 3}
```

```
int targetSum(vector<int> arr, int target)
{
  int n = arr.size();
  //int dp[n + 1][target + 1];
  vector<vector<int>> dp(n+1,vector<int>(target+1));//Better Declaration
  int totalSum = 0;
  for (int i = 0; i < n; i++)
  {
    totalSum += arr[i];
  }
  if (target > totalSum)
  {
    return 0;
  }
  for (int i = 0; i < n + 1; i++)
  {
    for (int j = 0; j < target + 1; j++)
    {
      if (i == 0)
      {
        dp[i][j] = 0;
      }
      if (j == 0)
      {
        dp[i][j] = 1;
      }
    }
  }
  for (int i = 1; i < n + 1; i++)
  {
    for (int j = 0; j < target + 1; j++)
    {
      if (arr[i - 1] <= j)
      {
        dp[i][j] = (dp[i - 1][j - arr[i - 1]] % MOD + dp[i - 1][j] % MOD) % MOD;
      }
      else
      {
        dp[i][j] = dp[i - 1][j] % MOD;
      }
    }
  }
  return dp[n][target];
}
``` 

## 5. Minimum Subset Sum Difference
minimum difference of sum of all elements of the two subsets
```
int minSubsetSumDifference(vector<int>arr)
{
  int n=arr.size();
  int totalSum=0;
  for(int i=0;i<n;i++)
  {
    totalSum+=arr[i];
  }
  bool dp[n+1][totalSum+1];
  for(int i=0;i<n+1;i++)
  {
    for(int j=0;j<totalSum+1;j++)
    {
      if(i==0)
      {
        dp[i][j]=false;
      }
      if(j==0)
      {
        dp[i][j]=true;
      }
    }
  }
  for(int i=1;i<n+1;i++){
    for(int j=1;j<totalSum+1;j++)
    {
      if(arr[i-1]<=j)
      {
        dp[i][j]=dp[i-1][j-arr[i-1]]||dp[i-1][j];
      }
      else{
        dp[i][j]=dp[i-1][j];
      }
    }
  }
  int sum=totalSum/2;
  int leftSum=INT_MIN;
  for(int j=0;j<=sum;j++)
  {
    if(dp[n][j]==true)
    {
      leftSum=max(leftSum,j);
    }
  }
  int rightSum=totalSum-leftSum;
  return abs(rightSum-leftSum);
}
``` 
## 6. Count Of Partitions with Given Difference :
'Array' and 'diff' given.How many ways we can make 2 subsets whose sum difference is 'diff'.
Input: N = 5, arr[] = [1, 2, 3, 1, 2], diff = 1
Output: 5
Explanation: We can have five partitions which is shown below
{1, 3, 1} and {2, 2} – S1 = 5, S2 = 4
{1, 2, 2} and {1, 3} – S1 = 5, S2 = 4
{3, 2} and {1, 1, 2} – S1 = 5, S2 = 4
{1, 2, 2} and {1, 3} – S1 = 5, S2 = 4
{3, 2} and {1, 1, 2} – S1 = 5, S2 = 4
Approach :
rightSum=totalSum-leftSum
rightSum=leftSum-diff
leftSum=(diff+totalSum)/2
so we will basically calculate no of subsets to get leftSum.
```
int countPartitions(int n, int d, vector<int> &arr)
{
  int totalSum = 0;
  for (int i = 0; i < n; i++)
  {
    totalSum = (totalSum + arr[i]) % MOD;
  }
  if ((totalSum + d) % 2 != 0 || totalSum < d)
    return 0;
  int sum1 = (totalSum + d) / 2;
  sum1 = min(sum1, totalSum - sum1);
  int target = sum1;
  return targetSum(arr, target);
}
```


## 7. Unbounded KnapSack (Working)
. We can consider any elements any number of times.(Rod Cutting problem,Coin exchange problem).
```
int unboundedKnapsack(vector<int> weight, vector<int> val, int Weight)
{
  int n = weight.size();
  int dp[n + 1][Weight + 1];
  for (int i = 0; i < n + 1; i++)
  {
    for (int j = 0; j < Weight + 1; j++)
    {
      if (i == 0 || j == 0)
      {
        dp[i][j] = 0;
      }
    }
  }
  for (int i = 1; i < n + 1; i++)
  {
    for (int j = 1; j < Weight+1; j++)
    {
      if (weight[i - 1] <= j)
      {
        dp[i][j] = max(val[i - 1] + dp[i][j - weight[i - 1]], dp[i - 1][j]);
      }
      else
      {
        dp[i][j] = dp[i - 1][j];
      }
    }
  }
  return dp[n][Weight];
}
```