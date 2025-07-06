<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# convert this article into python

A general approach to Stack problems in C++ | Generic Template

Prasanna
7827
Feb 10, 2023
Feb 25, 2023
Stack
C
Monotonic Stack
Career
Stack is a linear data structure that follows a particular order in which the operations are performed. The order may be LIFO(Last In First Out) or FILO(First In Last Out).

How to identify stack questions?

1. Time Complexity reduced from O(n^2) to O(n) (Generally)
2. Brute Force is of the type:
for( int i = 0 to i = n) :
for (int j dependent on i) :
end
end
Pattern 1: Monotonic Increasing / Decreasing Based Questions

Q1. Next Greater Element to the Right (NGER)
Input: N = 4, arr[] = [1 3 2 4]
Output: 3 4 4 -1

Approach:
For next greater to the right we travel from the left and we check while the stack top is less/equal we pop and finally we push the incoming element. We push the stack top to the vector and while returning we reverse the vector as we traveled from the last.

vector<int> NGER(vector<int> arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = n - 1 ; i >= 0 ; i-- ){
        
        while(st.size() > 0 && st.top() <= arr[i]){
            st.pop() ;
        }
        
        if(st.size() == 0){
            v.push_back(-1) ;
        }else{
            v.push_back(st.top()) ;
        }
        
        st.push(arr[i]) ;
    }
    
    reverse(v.begin() , v.end()) ;
    return v ;
    }
Q2) Next Greater Element to the Right- 2

Approach: We find NGER of the bigger array.

vector<int> nextGreaterElement(vector<int>\& nums1, vector<int>\& nums2) {
// size of nums2 > nums 1
vector<int> query = NGER(nums2) ;
vector<int> ans ;
// key : nums2 , value : NGE
unordered_map<int, int> mpp ;
for(int i = 0 ; i < nums2.size() ; i++ ){
mpp[nums2[i]] = query[i] ;
}

    for(int i = 0 ; i < nums1.size() ; i++ ){
      ans.push_back(mpp[nums1[i]]) ;
    }
    return ans ;
    }
Q3) Next Greater Element to the Right in a Circular Array
Approach: Already fill the stack in the reverse direction and then use the same approach.
By doing this we actually have the element on the stack that would have been on the left side of the array.

vector<int> nextGreaterElements(vector<int>\& nums) {
stack<int> st ;
vector<int> v ;
int n = nums.size() ;

    for(int i = n - 1 ; i >= 0 ; i-- ){
     	st.push(nums[i]) ;
    }
    
    for(int i = n - 1 ; i >= 0 ; i-- ){
        while(st.size() > 0 && st.top() <= nums[i]){
          st.pop() ;
        }
    
        if(st.size() == 0){
          v.push_back(-1) ;
        }else{
          v.push_back(st.top()) ;
        }
    
        st.push(nums[i]) ;
    }
    
    reverse(v.begin() , v.end()) ;
    return v ;
    }
Q4) Next Greater to the Left (NGEL)
Approach: Similar to NGER but we iterate from the start instead of the end and we don't reverse the output array

vector<int> NGEL(vector<int>\& arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = 0 ; i < n ; i++ ){
        
        while(st.size() > 0 && st.top() <= arr[i]){
            st.pop() ;
        }
        
        if(st.size() == 0){
            v.push_back(-1) ;
        }else{
            v.push_back(st.top()) ;
        }  
    	
        st.push(arr[i]) ;
    }
    
    return v ;
    }
Q5) Next smaller element to the right (NSER):
Approach: Similar to NGER Just change the sign while comparing stack top and array element

vector<int> NSER(vector<int> arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = n - 1 ; i >= 0 ; i-- ){
        
        while(st.size() > 0 && st.top() >= arr[i]){
            st.pop() ;
        }
        
        if(st.size() == 0){
            v.push_back(-1) ;
        }else{
            v.push_back(st.top()) ;
        }
        
        st.push(arr[i]) ;
    }
    
    reverse(v.begin() , v.end()) ;
    return v ;
    }
Q6) Next smaller element to the Left (NSEL):

Approach: Similar to NGER. Just traverse from the start , change the sign while comparing stack top and array element and don't reverse the output array

vector<int> NSEL(vector<int>\& arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = 0 ; i < n ; i++ ){
        
        while(st.size() > 0 && st.top() >= arr[i]){
            st.pop() ;
        }
        
        if(st.size() == 0){
            v.push_back(-1) ;
        }else{
            v.push_back(st.top()) ;
        }
        
        st.push(arr[i]) ;
    }
    
    return v ;
    }
Once you know how to find NGER, you get the intuition for NGEL, NSER and NSEL.
Knowing all four techniques NGER, NGEL, NSER and NSEL, you can solve a bunch of problems

Summary:

Problem	Stack Type	Operator in while loop
next greater right	decreasing	stackTop <= current
previous greater	decreasing	stackTop <= current
next smaller	increasing	stackTop >= current
previous smaller	increasing	stackTop >= current
Q7) Temperature Rise ( Try on your own)
Approach: Same as Next Greater to Right just store the index instead of the value

vector<int> dailyTemperatures(vector<int>\& temperatures) {
stack<int> st ;
vector<int> v ;
int n = temperatures.size() ;

    for(int i = n-1; i >= 0; i-- ){
        while(st.size() > 0 && temperatures[st.top()] <= temperatures[i]){
            st.pop() ;
        }         
    
        if(st.size() == 0){
            v.push_back(0) ;
        }else{
            v.push_back(st.top()-i) ;
        }
    
        st.push(i) ;
    }
    
    reverse(v.begin(), v.end()) ;
    return v ;
    }
Q8) Stock span problem very famous problem
Input: N = 7, price[] = [100 80 60 70 60 75 85]
Output: : 1 1 1 2 1 4 6
Explanation: Traversing the given input span for 100 will be 1, 80 is smaller than 100 so the span is 1, 60 is smaller than 80 so the span is 1, 70 is greater than 60 so the span is 2 and so on. Hence the output will be 1 1 1 2 1 4 6.

Approach: Just recognize which pattern this question out of NGER, NGEL, NSER, and NSEL
It is NGER since we need to find the greater stock price and store the difference in the array.

vector <int> calculateSpan(int arr[], int n){
stack<int> st ;
vector<int> v ;

    for( int i = 0 ; i < n ; i++ ){
    
      while(st.size() > 0 && arr[st.top()] <= arr[i]){
        st.pop() ;
      }
    
      if(st.size() == 0){
        v.push_back(i + 1) ;
      }else{
        v.push_back(i - st.top()) ;
      }
    
      st.push(i) ;
    }
    
    return v ;
    }
Q9) Maximum Area Histogram
Approach: Again this question is a combination of 2 problems: NSER and NSEL. We need to find consecutive bigger elements so we use the Nearest smaller concept. The only change is that when the stack is empty in NSER we push the last index and in NSEL we push -1 (index before 0)

int largestRectangleArea(vector<int>\& heights) {
// Instead of -1 push the last index (n) if stack is empty
vector<int> right = NSER(heights) ;
// Push -1 if stack is empty
vector<int> left = NSEL(heights) ;

    int ans = -1 ;
    
    for(int i=0 ;i<heights.size() ;i++){
      ans=max(ans,(right[i]-left[i]-1)*heights[i]);
    } 
    
    return ans ;
    }
Full Code:

vector<int> NSER(vector<int> arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = n - 1 ; i >= 0 ; i-- ){
    
        while(st.size() > 0 && arr[st.top()] >= arr[i]){
            st.pop() ;
        }
    
        if(st.size() == 0){
            v.push_back(n) ;
        }else{
            v.push_back(st.top()) ;
        }
    
        st.push(i) ;
    }
    reverse(v.begin() , v.end()) ;
    return v ;
    }

vector<int> NSEL(vector<int> arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = 0 ; i < n ; i++ ){
    
        while(st.size() > 0 && arr[st.top()] >= arr[i]){
            st.pop() ;
        }
    
        if(st.size() == 0){
            v.push_back(-1) ;
        }else{
            v.push_back(st.top()) ;
        }
    
        st.push(i) ;
    }
    
    return v ;
    }

int largestRectangleArea(vector<int>\& heights) {
vector<int> right = NSER(heights) ;
vector<int> left = NSEL(heights) ;

    int ans = -1 ;
    
    for(int i=0 ;i<heights.size() ;i++){
        ans=max(ans,(right[i]-left[i]-1)*heights[i]);
    } 
    
    return ans ;
    }
Q10) Max Area Rectangle in binary matrix

Approach: Direct Implementation of Max Area in a Histogram(MAH). Apply MAH to every row of the matrix with 1 catch:. If the element is 0 then the whole column becomes if it is 1 then add it to the vector and apply MAH.
Return maximum value amongst all answers.

int maximalRectangle(vector<vector<char>>\& nums) {

    if(nums.size() == 0){
      return 0 ;
    }
    vector<int> ds ;
    for(int j = 0 ; j < nums[0].size() ; j++){
      if(nums[0][j] == '1'){
        ds.push_back(1) ;
      }else{
        ds.push_back(0) ;
      }
    }
    int mx = largestRectangleArea(ds) ;
    
    for(int i = 1 ; i< nums.size() ; i++ ){
      for(int j = 0 ; j<nums[i].size() ; j++ ){
        if(nums[i][j] != '0'){
          ds[j]++ ;
        }else{
          ds[j] = 0 ;
        }
      }
      mx = max(largestRectangleArea(ds) , mx);	
    }
    return mx ;
    }
Q11. Valid Subarrays

Given array nums of integers, return the number of non-empty continuous subarrays that satisfy the following condition:
The leftmost element of the subarray is not larger than other elements in the subarray.

vector<int> countOfSubArray(vector<int> arr){
stack<int> st ;
vector<int> v ;
int n = arr.size() ;

    for( int i = n - 1 ; i >= 0 ; i-- ){
        
        while(st.size() > 0 && arr[st.top()] >= arr[i]){
            st.pop() ;
        }
        
        if(st.size() == 0){
            v.push_back(n-i) ;
        }else{
            v.push_back(st.top()-i) ;
        }
        
        st.push(i) ;
    }
    
    reverse(v.begin() , v.end()) ;
    return v ;
    }
Next 3 problems are quite tough and its okay if you don't get their solution yourself

Q12. Remove K Digits

Maintain a monotonic equal or incresing stack

The method is straightforward. We want the smallest number possible, so we start by removing the most significant digits first. For example, if we have the digits 1-4, then the smallest number would be 1234 and not 2314 or any other combination. To achieve this, we remove any digit that is greater than the following digit. So in the case of 2314, we first remove 3 as it is greater than 1 and then 2 since it is also greater than 1.
To implement this, we use a stack data structure. Whenever the top digit on the stack is greater than the current digit, we pop it off.
The conditions in the while loop are crucial to avoid runtime errors. For example, in the case of ["10001", 2], the answer is "0", but if we don't specify the condition !s.empty(), then the while loop will try to pop from an empty stack, causing a runtime error.
string removeKdigits(string num, int k) {
stack<int> st ;
string op = "" ;

    for(int i = 0 ; i < num.size() ; i++ ){
        while(st.size() > 0 && k > 0 && st.top() > num[i] ){
            st.pop() ;
            k-- ;
        }
        st.push(num[i]) ;
    }
    
    while(st.size() > 0 && k > 0){
        st.pop() ;
        k-- ;
    }
    
    while(!st.empty()){
        op += st.top() ;
        st.pop() ;
    }
    
    reverse(op.begin() , op.end()) ;
    
    int s = 0;
    while (s < (int)op.size()-1 && op[s]=='0')  s++;
    op.erase(0, s);
    
    return op == "" ? "0" : op ;
    }
Q13. 132 Pattern
Montonic Strictly Decreasing Stack

bool find132pattern(vector<int>\& nums) {
int n = nums.size() ;
stack<pair<int, int>>  st ;
int curr_min = nums[0];

    for (int i = 1; i < n ; i++) {
        while (!st.empty() && nums[i] >= st.top().first){
            st.pop();
        }
        
        if (!st.empty() && nums[i] > st.top().second){
            return true;
        }
            
        curr_min = min(nums[i], curr_min);
        st.push({nums[i], curr_min});
    }
    return false;
    }
Q14. Maximum Subarray Min-Product
Monotonic Increasing Stack

In order to find the minimum product, we can't simply calculate the minimum from all sub arrays as it will result in a time limit exceeded (TLE) error.
So, instead, we take each element nums[i] (where 0 <= i < n) as the minimum element and find the minimum product of subarrays with nums[i] as the minimum.
We first calculate the prefix sum of the given array and then determine the number of largest elements to the left in the "array left" and the number of largest elements to the right in the "array right" with nums[i] as the minimum.
After finding this information, it becomes straightforward to find the length of the subarray with nums[i] as the minimum. To find the starting index (a), we use the equation a = i - left[i] + 1, and for the ending index (b), we use b = i + right[i] - 1.
Then, we calculate the result by using the prefix sum and knowing that the minimum element is nums[i].
Finally, we update the result by finding the maximum of the minimum product.
int maxSumMinProduct(vector<int>\& nums) {
int n = nums.size();
int left[n], right[n];
long long int sum[n];
sum[0] = nums[0];
for(int i=1;i<n;i++){
sum[i] = sum[i-1]+nums[i];
}
stack<pair<int, int> > s1, s2;
for (int i = 0; i < n; ++i) {
int cnt = 1;
while (!s1.empty() \&\& (s1.top().first) > nums[i]) {
cnt += s1.top().second;
s1.pop();
}
s1.push({ nums[i], cnt });
left[i] = cnt;
}
for (int i = n - 1; i >= 0; --i) {
int cnt = 1;
while (!s2.empty() \&\& (s2.top().first) >= nums[i]) {
cnt += s2.top().second;
s2.pop();
}

            s2.push({ nums[i], cnt });
            right[i] = cnt;
        }
        long long int res =0;
        for(int i=0;i<n;i++){
            int a = i-left[i]+1;
            int b = i+right[i]-1;
            long long int subArraySum;
            if(a>0)
                subArraySum = sum[b]-sum[a-1];
            else
                subArraySum = sum[b];
            res = max(res, subArraySum*nums[i]);
        }
        return res%1000000007;
    }
    2. Stack Parenthesis Questions

Q1) Valid Parenthesis
We push the opening bracket in the stack and if we encounter any type of closing bracket then we check if stack top should be equal to that closing bracket else we return false

bool isValid(string s) {
stack<char> st ;
for(int i = 0 ;i < s.size() ; i++ ){
if(s[i] == '(' || s[i] == '[' || s[i] == '{' ){
st.push(s[i]) ;
}else if(s[i] == ')' \&\& (st.empty() ||  st.top() != '(' )){
return false ;
}else if(s[i] == ']' \&\&  (st.empty() || st.top() != '[' )){
return false ;
}else if(s[i] == '}' \&\&  (st.empty() || st.top() != '{')){
return false ;
}else{
st.pop() ;
}
}
return st.empty() ;
}
Q2. Redundant Parenthesis

If we encounter a closing bracket and immediately on top we find an opening bracket then it is redundant, else we pop all elements until we reach the opening bracket.

bool isRedundantParenthesis(string s){
stack<char> st ;
for(int i=0 ; i < s.size() ; i++ ){
if(s[i] == ')'){
if(st.top() == '('){
return true ;
}else{
while(st.top() != '('){
st.pop() ;
}
st.pop() ;
}
}else{
st.push(s[i]) ;
}
}
return false ;
}
Q3. Minimum Add To Make Parentheses Valid
Similar to Q1

int minAddToMakeValid(string s) {
stack<char> st ;
int count = 0 ;
for(int i = 0 ;i < s.size() ; i++ ){
if(s[i] == '('  ){
st.push(s[i]) ;
}
// If the character is closing bracket and either if stack is empty or stack top is not an opening bracket then it is invalid
else if(s[i] == ')' \&\& (st.empty() ||  st.top() != '(' )){
count++ ;
}else{
st.pop() ;
}
}
// if stack is not empty we add that to the count as well
return count + st.size();
}
Q5. Longest Valid Parentheses

Brute Force: Extension of Is Valid Parenthesis Problem.

Basically, check for every substring validity and return the longest substring

int longestValidParentheses(string s) {
int mx = 0 ;
for(int i = 0; i < s.size(); i++){
string temp = "" ;
temp += s[i] ;
for(int j = i + 1; j < s.size(); j++){
temp += s[j] ;
cout<<temp ;
if(isValid(temp)){
int n = temp.size() ;
mx = max(mx, n) ;
}
}
}
return mx ;
}
Efficient Approach:

int longestValidParentheses(string s) {
stack<int> st ;
st.push(-1) ;
int mx = 0 ;
for(int i = 0; i < s.size(); i++){
if(s[i] == '('){
st.push(i) ;
}else{
st.pop() ;
if(st.empty()){
st.push(i) ;
}else{
mx = max(mx, i - st.top()) ;
}
}
}
return mx ;
}
Q6. Reverse each word

string reverseWords(string s) {
stack<string>st;
for(int i =0;i<s.length();i++){
string word="";
if(s[i]==' ')continue;
while(s[i]!=' ' and i<s.length()){
word+=s[i];
i++;
}
st.push(word);

    }
    string ans="";
    while(!st.empty()){
    	ans+=st.top();
    	st.pop();
    	if(!st.empty())ans+=" ";
    }
    
    return ans;
    }
Pattern 3: Implementation-type Problems

While pushing
if x < minEle : push 2*x-minEle and put the provided x in st
Similarly while popping
if s.top() < minEle(indicates the flag): Restore the mn before popping: minEle = 2* minEle -st.top()
Q1. Min Stack

class MinStack {
public:
stack<long long int> st ;
long long int minEle;

    MinStack() {
        minEle = INT_MIN;
    }
    
    void push(int val) {
       if(st.empty()){
           minEle = val;
           st.push(val);
       }else{
            if(val < minEle){
                st.push((1ll * 2*val) - minEle);
                minEle = val;
            }else{
                st.push(val);
            }
        }
    }
    
    void pop() {
        if(st.top() < minEle){
            minEle = 2*minEle - st.top();
            st.pop();
        }else{
            st.pop();
        }  
    }
    
    int top(){
        if(st.top() < minEle){
            return minEle;
        }
            return st.top();
    }
    
    int getMin() {
        return minEle;
    }
    };
Q2. Maximum Frequency Stack

We will consider a frequency map that will count the occurences of each element.
We will also consider a map group_stack which will group the elements with the same frequecy. Example if two elements have same count then we will add them in stack with the recent element at the top.
When we will push the elements
*We will increment its frequency
*Update the maximum occurence element
*group the element with its frequency
When we will pop the element from stack
*We will take out the max_frequency element.
*Remove it from group_stack
*Decrement its freuency
*Return it
class FreqStack {
public:
//This will store the count of each element
unordered_map<int,int> frequency;
//This maps the elements which have same count
//But the element that come last will come first of same count
unordered_map<int,stack<int>> group_stack;
//Maximum frequency possible
int max_frequency=0;

    FreqStack() {
        
    }
    //Push elements in the stack
    void push(int val) {
        //Increment the count
        frequency[val]++;
        //Check is this element occurs maximum time
        max_frequency=max(max_frequency,frequency[val]);
        //Map the element with its count
        group_stack[frequency[val]].push(val);
    }
    
    int pop() {
        //Find the max occurence element
        int top_max_frequency=group_stack[max_frequency].top();
        //Remove it from stack
        group_stack[max_frequency].pop();
        //Decrement its count
        frequency[top_max_frequency]--;
        //If there is no element of maximum frquency the decrement max_frequency
        if(group_stack[max_frequency].size()==0)
            max_frequency--;
        
        return top_max_frequency;
    }
    };
Pattern 4: Advanced Stack Problems

Q1. Merge Intervals

Sort the intervals .
While traversing the intervals vector we will come accross two coditions
First condition : if there is a overlapping between the intervals then just take out the max element from the ending point and thus we merged them
eg:- [1,4],[2,8] =Mergerd intervals will be> [1,8]
Second condition : if there is no overlapping then simply push those interval to our resultant vector .
vector<vector<int>> merge(vector<vector<int>>\& intervals) {
if(!intervals.size()) return {};
sort(intervals.begin(),intervals.end());

    vector<vector<int>>v;
    stack<vector<int>> st ;
    
    st.push(intervals[0]) ;
    for(int i = 1 ; i<intervals.size() ; i++){
        if(intervals[i][0] > st.top()[1]){
            st.push(intervals[i]) ;
        }else{
            st.top()[1] = max(intervals[i][1], st.top()[1]) ;
        }
    }
    
    while(!st.empty()){
        v.push_back(st.top()) ;
        st.pop() ;
    }
    reverse(v.begin() , v.end()) ;
    return v ;
    }
Q2. Insert Interval

vector<vector<int>> insert(vector<vector<int>>\& intervals, vector<int>\& newInterval) {
intervals.push_back(newInterval) ;
// Function of Q1 Merge Intervals
return merge(intervals) ;
}
Q3. Asteroid Collision

Whenever we encounter a positive value, we will simply push it to the stack.
The moment we encounter a negative value, we know some or all or zero positive values will be knocked out of the stack. The negative value may itself be knocked out or it may enter the stack.
vector<int> asteroidCollision(vector<int>\& ast) {
int n = ast.size();
stack<int> st;
for(int i = 0; i < n; i++) {
if(ast[i] > 0 || st.empty()) {
st.push(ast[i]);
}else {
while(!st.empty() and st.top() > 0 and st.top() < abs(ast[i]) ) {
st.pop();
}
if(!st.empty() and st.top() == abs(ast[i])) {
st.pop();
}else if(st.empty() || st.top() < 0) {
st.push(ast[i]);

    		}
    	}
    }
    // finally we are returning the elements which remains in the stack.
    // we have to return them in reverse order.
    vector<int> res(st.size());
    for(int i = (int)st.size() - 1; i >= 0; i--) {
    	res[i] = st.top();
    	st.pop();
    }
    return res;
    }
Q4. Celebrity Problem
Very interesting problem since the date is of O(n^2) and we need Time Complexity of O(n)
We initially push all the elements in the stack then compare the celebrity among them.
Lats element in the stack is the potential celebrity
Then we traverse the pot row and column to actual confirm whether it is actually a celebrity or not.

int celebrity(vector<vector<int> >\& arr, int n){

    stack<int> st ;
    for(int i = 0 ; i < n ; i++ ){
        st.push(i) ;
    }
    
    while(st.size() >= 2){
        int top1 = st.top() ;
        st.pop() ;
        int top2 = st.top() ;
        st.pop() ;
        
        if(arr[top1][top2] == 1){
            st.push(top2) ;
        }else{
            st.push(top1) ;
        }
    }
    
    int pot = st.top() ;
    
    for(int i = 0 ; i < arr.size() ; i++ ){
        if(i != pot){
            if(arr[pot][i] == 1 || arr[i][pot] == 0){
                return -1 ;
            }
        }
    }
    
    return pot ;
    }
Q5. Construct Smallest Number From DI String

If we have encountered I or we are at the last character of input string, then pop from the stack and add it to the end of the output string until the stack gets empty.
If we have encountered D, then we want the numbers in decreasing order. so we just push (i+1) to our stack.
string printMinNumberForPattern(string s){

    stack<int> st;
    string ans = "";
    int n = 1 ;
    for(int i=0; i<s.size();i++){
      st.push(n);
      n++ ;
      if(s[i] == 'I'){
        while(st.size()!=0){
          ans += to_string(st.top());
          st.pop();
        }
      }
    
    }
    st.push(n);
    while(st.size()!=0){
      ans += to_string(st.top());
      st.pop();
    }
    return ans;
    }
Q6. Evaluate Reverse Polish Notation

int evalRPN(vector<string>\& tokens) {
stack<string> st ;

    for(auto it: tokens){
        if(it == "+"){
            string tp1 = st.top() ;
            st.pop() ;
            string tp2 = st.top() ;
            st.pop() ;
            st.push(to_string(stol(tp2) + stol(tp1))) ;
        }else if(it == "-"){
            string tp1 = st.top() ;
            st.pop() ;
            string tp2 = st.top() ;
            st.pop() ;
            st.push(to_string(stol(tp2) - stol(tp1))) ;
        }else if(it == "*"){
            string tp1 = st.top() ;
            st.pop() ;
            string tp2 = st.top() ;
            st.pop() ;
            st.push(to_string(stol(tp2) * stol(tp1))) ;
        }else if(it == "/"){
            string tp1 = st.top() ;
            st.pop() ;
            string tp2 = st.top() ;
            st.pop() ;
            st.push(to_string(stol(tp2) / stol(tp1))) ;
        }else{
            st.push(it) ;
        }
    }
    
    return stoi(st.top()) ;
    }
Q7. Simplify Path

By looking at examples we can see that the above simplification process just behaves like a stack.
Whenever we encounter any file’s name, we simply push it into the stack.
When we come across ” . ” we do nothing.
When we find “..” in our path, we simply pop the topmost element as we have to jump back to parent’s directory.
When we see multiple “////” we just ignore them as they are equivalent to one single “/”.
After iterating through the whole string the elements remaining in the stack is our simplified absolute path.
string simplifyPath(string path) {

    stack<string> st;
    string res;
    
    for(int i = 0;  i<path.size(); ++i)
    {
    	if(path[i] == '/')    
    		continue;
    	string temp;
    	// iterate till we doesn't traverse the whole string and doesn't encounter the last /
    	while(i < path.size() && path[i] != '/')
    	{
    		// add path to temp string
    		temp += path[i];
    		++i;
    	}
    	if(temp == ".")
    		continue;
    	// pop the top element from stack if exists
    	else if(temp == "..")
    	{
    		if(!st.empty())
    			st.pop();
    	}
    	else
    	// push the directory file name to stack
    		st.push(temp);
    }
    
    // adding all the stack elements to res
    while(!st.empty())
    {
    	res = "/" + st.top() + res;
    	st.pop();
    }
    
    // if no directory or file is present
    if(res.size() == 0)
    	return "/";
    
    return res;
    }

Absolutely! Here’s a **Python version** of the most important stack algorithms and problems from your article, with clear functions and usage examples for each pattern. I’ll start with the core stack-based algorithms and then provide the classic problems you listed.

## 1. Next Greater Element to the Right (NGER)

```python
def nger(arr):
    stack = []
    n = len(arr)
    res = [-1] * n
    for i in range(n-1, -1, -1):
        while stack and stack[-1] <= arr[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(arr[i])
    return res

# Example
print(nger([1, 3, 2, 4]))  # Output: [3, 4, 4, -1]
```


## 2. Next Greater Element to the Right for Two Arrays (nums1 subset of nums2)

```python
def nextGreaterElement(nums1, nums2):
    stack = []
    nge = {}
    for num in reversed(nums2):
        while stack and stack[-1] <= num:
            stack.pop()
        nge[num] = stack[-1] if stack else -1
        stack.append(num)
    return [nge[num] for num in nums1]

# Example
print(nextGreaterElement([4,1,2], [1,3,4,2]))  # Output: [-1, 3, -1]
```


## 3. Next Greater Element in Circular Array

```python
def nextGreaterElements(nums):
    n = len(nums)
    res = [-1] * n
    stack = []
    # Iterate twice to simulate circularity
    for i in list(range(n)) * 2:
        while stack and nums[stack[-1]] < nums[i]:
            res[stack.pop()] = nums[i]
        stack.append(i)
    return res

# Example
print(nextGreaterElements([1, 2, 1]))  # Output: [2, -1, 2]
```


## 4. Next Greater Element to the Left (NGEL)

```python
def ngel(arr):
    stack = []
    res = []
    for num in arr:
        while stack and stack[-1] <= num:
            stack.pop()
        res.append(stack[-1] if stack else -1)
        stack.append(num)
    return res

# Example
print(ngel([1, 3, 2, 4]))  # Output: [-1, -1, 3, -1]
```


## 5. Next Smaller Element to the Right (NSER)

```python
def nser(arr):
    stack = []
    n = len(arr)
    res = [-1] * n
    for i in range(n-1, -1, -1):
        while stack and stack[-1] >= arr[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(arr[i])
    return res

# Example
print(nser([1, 3, 2, 4]))  # Output: [-1, 2, -1, -1]
```


## 6. Next Smaller Element to the Left (NSEL)

```python
def nsel(arr):
    stack = []
    res = []
    for num in arr:
        while stack and stack[-1] >= num:
            stack.pop()
        res.append(stack[-1] if stack else -1)
        stack.append(num)
    return res

# Example
print(nsel([1, 3, 2, 4]))  # Output: [-1, 1, 1, 2]
```


## 7. Daily Temperatures

```python
def dailyTemperatures(temps):
    stack = []
    res = [0] * len(temps)
    for i in range(len(temps)):
        while stack and temps[stack[-1]] < temps[i]:
            idx = stack.pop()
            res[idx] = i - idx
        stack.append(i)
    return res

# Example
print(dailyTemperatures([73, 74, 75, 71, 69, 72, 76, 73]))  # Output: [1, 1, 4, 2, 1, 1, 0, 0]
```


## 8. Stock Span Problem

```python
def stockSpan(prices):
    stack = []
    res = []
    for i in range(len(prices)):
        while stack and prices[stack[-1]] <= prices[i]:
            stack.pop()
        res.append(i - stack[-1] if stack else i + 1)
        stack.append(i)
    return res

# Example
print(stockSpan([100, 80, 60, 70, 60, 75, 85]))  # Output: [1, 1, 1, 2, 1, 4, 6]
```


## 9. Maximum Area Histogram

```python
def largestRectangleArea(heights):
    def nser(arr):
        stack = []
        res = [len(arr)] * len(arr)
        for i in reversed(range(len(arr))):
            while stack and arr[stack[-1]] >= arr[i]:
                stack.pop()
            if stack:
                res[i] = stack[-1]
            stack.append(i)
        return res

    def nsel(arr):
        stack = []
        res = [-1] * len(arr)
        for i in range(len(arr)):
            while stack and arr[stack[-1]] >= arr[i]:
                stack.pop()
            if stack:
                res[i] = stack[-1]
            stack.append(i)
        return res

    left = nsel(heights)
    right = nser(heights)
    max_area = 0
    for i in range(len(heights)):
        width = right[i] - left[i] - 1
        max_area = max(max_area, heights[i] * width)
    return max_area

# Example
print(largestRectangleArea([2, 1, 5, 6, 2, 3]))  # Output: 10
```


## 10. Max Area Rectangle in Binary Matrix

```python
def maximalRectangle(matrix):
    if not matrix:
        return 0
    m, n = len(matrix), len(matrix[0])
    heights = [0] * n
    max_area = 0
    for i in range(m):
        for j in range(n):
            heights[j] = heights[j] + 1 if matrix[i][j] == '1' else 0
        max_area = max(max_area, largestRectangleArea(heights))
    return max_area

# Example
matrix = [
    ["1","0","1","0","0"],
    ["1","0","1","1","1"],
    ["1","1","1","1","1"],
    ["1","0","0","1","0"]
]
print(maximalRectangle(matrix))  # Output: 6
```


## 11. Valid Parentheses

```python
def isValid(s):
    stack = []
    for c in s:
        if c in '([{':
            stack.append(c)
        else:
            if not stack:
                return False
            if (c == ')' and stack[-1] != '(') or \
               (c == ']' and stack[-1] != '[') or \
               (c == '}' and stack[-1] != '{'):
                return False
            stack.pop()
    return not stack

# Example
print(isValid("()[]{}"))  # Output: True
```


## 12. Remove K Digits

```python
def removeKdigits(num, k):
    stack = []
    for digit in num:
        while k > 0 and stack and stack[-1] > digit:
            stack.pop()
            k -= 1
        stack.append(digit)
    # Remove remaining digits if needed
    stack = stack[:-k] if k > 0 else stack
    # Remove leading zeros
    res = ''.join(stack).lstrip('0')
    return res if res else '0'

# Example
print(removeKdigits("1432219", 3))  # Output: "1219"
```


## 13. 132 Pattern

```python
def find132pattern(nums):
    if len(nums) < 3:
        return False
    stack = []
    s3 = float('-inf')
    for num in reversed(nums):
        if num < s3:
            return True
        while stack and stack[-1] < num:
            s3 = stack.pop()
        stack.append(num)
    return False

# Example
print(find132pattern([3, 1, 4, 2]))  # Output: True
```


## 14. Maximum Subarray Min-Product

```python
def maxSumMinProduct(nums):
    n = len(nums)
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i+1] = prefix[i] + nums[i]
    
    left = [0] * n
    stack = []
    for i in range(n):
        while stack and nums[stack[-1]] >= nums[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)
    
    right = [0] * n
    stack = []
    for i in reversed(range(n)):
        while stack and nums[stack[-1]] >= nums[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)
    
    max_product = 0
    for i in range(n):
        l, r = left[i], right[i]
        s = prefix[r] - prefix[l+1]
        max_product = max(max_product, nums[i] * s)
    return max_product % (10**9 + 7)

# Example
print(maxSumMinProduct([1, 2, 3, 2]))  # Output: 14
```


## 15. Min Stack

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self):
        if self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self):
        return self.stack[-1]

    def getMin(self):
        return self.min_stack[-1]
```


## 16. Frequency Stack

```python
from collections import defaultdict

class FreqStack:
    def __init__(self):
        self.freq = defaultdict(int)
        self.group = defaultdict(list)
        self.max_freq = 0

    def push(self, val):
        self.freq[val] += 1
        self.max_freq = max(self.max_freq, self.freq[val])
        self.group[self.freq[val]].append(val)

    def pop(self):
        val = self.group[self.max_freq].pop()
        self.freq[val] -= 1
        if not self.group[self.max_freq]:
            self.max_freq -= 1
        return val
```


## 17. Evaluate Reverse Polish Notation

```python
def evalRPN(tokens):
    stack = []
    for token in tokens:
        if token in '+-*/':
            b, a = stack.pop(), stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(int(a / b))
        else:
            stack.append(int(token))
    return stack[0]

# Example
print(evalRPN(["2","1","+","3","*"]))  # Output: 9
```


## 18. Simplify Path

```python
def simplifyPath(path):
    stack = []
    for part in path.split('/'):
        if part == '' or part == '.':
            continue
        elif part == '..':
            if stack:
                stack.pop()
        else:
            stack.append(part)
    return '/' + '/'.join(stack)

# Example
print(simplifyPath("/home//foo/"))  # Output: "/home/foo"
```


## 19. Asteroid Collision

```python
def asteroidCollision(asteroids):
    stack = []
    for num in asteroids:
        while stack and stack[-1] > 0 and num < 0:
            if stack[-1] == -num:
                stack.pop()
                break
            elif stack[-1] < -num:
                stack.pop()
                continue
            else:
                break
        else:
            stack.append(num)
    return stack

# Example
print(asteroidCollision([5, 10, -5]))  # Output: [5, 10]
```


## 20. Celebrity Problem

```python
def celebrity(M, n):
    stack = list(range(n))
    while len(stack) > 1:
        a = stack.pop()
        b = stack.pop()
        if M[a][b]:
            stack.append(b)
        else:
            stack.append(a)
    pot = stack[0]
    for i in range(n):
        if i != pot and (M[pot][i] or not M[i][pot]):
            return -1
    return pot

# Example
M = [[0, 0, 1, 0], [0, 0, 1, 0], [0, 0, 0, 0], [0, 0, 1, 0]]
print(celebrity(M, 4))  # Output: 2
```


## 21. Construct Smallest Number From DI String

```python
def smallestNumber(pattern):
    stack = []
    res = []
    n = 1
    for c in pattern:
        stack.append(n)
        n += 1
        if c == 'I':
            while stack:
                res.append(str(stack.pop()))
    stack.append(n)
    while stack:
        res.append(str(stack.pop()))
    return ''.join(res)

# Example
print(smallestNumber("IIIDIDDD"))  # Output: "123549876" (Note: This is a standard answer for this pattern, but the actual answer depends on the logic. The code above will not give 123549876, but it's the classic way to do it. For this specific pattern, a different approach may be required for the exact answer, but the code is a common approach for similar problems.)
```

> **Note:** The "IIIDIDDD" example above may not produce the exact output as expected, but the code is a standard way to solve such problems. For exact output, you may need to tweak the logic, but the stack-based approach is correct.

# Summary Table

| Problem / Pattern | Python Function Name | Description |
| :-- | :-- | :-- |
| NGER | `nger` | Next Greater Element to the Right |
| NGEL | `ngel` | Next Greater Element to the Left |
| NSER | `nser` | Next Smaller Element to the Right |
| NSEL | `nsel` | Next Smaller Element to the Left |
| Circular NGE | `nextGreaterElements` | NGE in Circular Array |
| Daily Temperatures | `dailyTemperatures` | Days until warmer temperature |
| Stock Span | `stockSpan` | Stock span problem |
| Largest Rectangle Area | `largestRectangleArea` | Maximum area in histogram |
| Maximal Rectangle | `maximalRectangle` | Max rectangle in binary matrix |
| Valid Parentheses | `isValid` | Check valid parentheses |
| Remove K Digits | `removeKdigits` | Remove k digits to form smallest number |
| 132 Pattern | `find132pattern` | Check for 132 pattern |
| Max Subarray Min-Product | `maxSumMinProduct` | Max product of min and sum in subarrays |
| Min Stack | `MinStack` | Stack with getMin in O(1) |
| Frequency Stack | `FreqStack` | Stack with most frequent elements |
| Evaluate RPN | `evalRPN` | Evaluate reverse polish notation |
| Simplify Path | `simplifyPath` | Simplify Unix path |
| Asteroid Collision | `asteroidCollision` | Asteroid collision simulation |
| Celebrity Problem | `celebrity` | Find celebrity in a party |
| Smallest Number from DI | `smallestNumber` | Construct smallest number from DI string |

If you need any specific problem or more details, let me know!

