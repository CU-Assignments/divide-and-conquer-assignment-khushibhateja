
# 1.Longest Nice Substring

CODE: 

class Solution {
public:
    string longestNiceSubstring(string s) {
        if (s.size() < 2) return "";
        
        unordered_set<char> st(s.begin(), s.end());
        for (int i = 0; i < s.size(); ++i) {
            if (st.count(tolower(s[i])) && st.count(toupper(s[i]))) 
            continue;
            string left = longestNiceSubstring(s.substr(0, i));
            string right = longestNiceSubstring(s.substr(i + 1));
            return left.size() >= right.size() ? left : right;
        }
        return s;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/5742f323-bb58-439c-8e8f-1cfae734f562)

# 2.Reverse Bits

CODE: 

class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t result = 0;
        for (int i = 0; i < 32; i++) {
            result = (result << 1) | (n & 1);
            n >>= 1;
        }
        return result;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/215d8d09-519b-4a82-9733-5bc5e7a6db20)

# 3.Number of 1 Bits

CODE: 

class Solution {
public:
    int hammingWeight(int n) {
        int count = 0;
        while (n) {
            n &= (n - 1); // Clears the lowest set bit
            count++;
        }
        return count;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/7eef4ea3-d2a0-457b-b0a1-c1f13b501782)

# 4.Maximum Subarray

CODE: 

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0], currentSum = nums[0];
        
        for (int i = 1; i < nums.size(); ++i) {
            currentSum = max(nums[i], currentSum + nums[i]);
            maxSum = max(maxSum, currentSum);
        }
        
        return maxSum;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/75519e19-cb6b-423e-bec6-35385c2ea9e3)

# 5.Search a 2D Matrix II

CODE: 

class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int row = 0, col = n - 1; 
        
        while (row < m && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target) {
                col--; 
            } else {
                row++; 
            }
        }
        
        return false;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/7fad5c24-9e7a-42f7-b94e-97a3af1860aa)

# 6.Super Pow

CODE: 

class Solution {
public:
    const int MOD = 1337;

    int modPow(int x, int y) {
        int res = 1;
        x %= MOD;
        while (y) {
            if (y % 2) res = res * x % MOD;
            x = x * x % MOD;
            y /= 2;
        }
        return res;
    }

    int superPow(int a, vector<int>& b) {
        int res = 1;
        for (int digit : b)
            res = modPow(res, 10) * modPow(a, digit) % MOD;
        return res;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/ab6d21be-8db4-4af6-bd93-7e16d109c997)

# 7.Beautiful Array

CODE: 

class Solution {
public:
    vector<int> beautifulArray(int n) {
        vector<int> res = {1};
        while (res.size() < n) {
            vector<int> temp;
            for (int num : res) if (num * 2 - 1 <= n) temp.push_back(num * 2 - 1);
            for (int num : res) if (num * 2 <= n) temp.push_back(num * 2);
            res = temp;
        }
        return res;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/edc27bb2-24f7-44b2-b009-9f8d8f277581)

# 8.The Skyline Problem

CODE: 

class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<pair<int, int>> events;
        for (auto& b : buildings) {
            events.emplace_back(b[0], -b[2]);
            events.emplace_back(b[1], b[2]);
        }
        sort(events.begin(), events.end());
        
        multiset<int> heights = {0};
        vector<vector<int>> skyline;
        int prevHeight = 0;
        
        for (auto& [x, h] : events) {
            if (h < 0) heights.insert(-h);
            else heights.erase(heights.find(h));
            int currHeight = *heights.rbegin();
            if (currHeight != prevHeight) {
                skyline.push_back({x, currHeight});
                prevHeight = currHeight;
            }
        }
        return skyline;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/7538c437-476e-4b42-bc4e-83a861d0478d)

# 9.Reverse Pairs

CODE: 

class Solution {
public:
    int reversePairs(vector<int>& nums) {
        return mergeSort(nums, 0, nums.size() - 1);
    }

    int mergeSort(vector<int>& nums, int l, int r) {
        if (l >= r) return 0;
        int mid = (l + r) / 2, count = mergeSort(nums, l, mid) + mergeSort(nums, mid + 1, r);
        int j = mid + 1;
        
        for (int i = l; i <= mid; i++) {
            while (j <= r && nums[i] > 2LL * nums[j]) j++;
            count += (j - (mid + 1));
        }

        inplace_merge(nums.begin() + l, nums.begin() + mid + 1, nums.begin() + r + 1);
        return count;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/763c370c-fb53-4ef9-bb36-f2363c381385)

# 10.Longest Increasing Subsequence II

CODE: 

#include <vector>
#include <map>
using namespace std;

class Solution {
public:
    int lengthOfLIS(vector<int>& nums, int k) {
        map<int, int> dp;
        int maxLen = 1;
        
        for (int num : nums) {
            int best = 0;
            for (auto it = dp.lower_bound(num - k); it != dp.upper_bound(num - 1); ++it)
                best = max(best, it->second);
            
            dp[num] = best + 1;
            maxLen = max(maxLen, dp[num]);
        }
        
        return maxLen;
    }
};

OUTPUT:

![image](https://github.com/user-attachments/assets/2bb8228d-33f7-4b93-b1cb-db394c739a34)














