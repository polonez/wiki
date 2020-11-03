---
layout  : wiki
title   : Palindrome
summary : Find the longest palindrome
date    : 2020-11-01 19:05:46 +0900
updated : 2020-11-03 15:22:57 +0900
tags    : palindrome algorithm
toc     : true
public  : true
parent  : algorithm
latex   : false
---
* TOC
{:toc}

# Dynamic Programming

O(n**2) algorithm

```swift
/// Check if the given string is palindrome
/// - Parameter string: string to check palindrome
/// - Returns: true if palindrome, otherwise false
///
/// Time complexity: O(n)
///
/// It calls itself recursively
func isPalindrome(string: String) -> Bool {
    guard string.count > 1 else { return true }
    guard string.first == string.last else { return false }
    return isPalindrome(string: String(string.dropFirst().dropLast()))
}

assert(isPalindrome(string: "aba") == true)
assert(isPalindrome(string: "abc") == false)
assert(isPalindrome(string: "abba") == true)
assert(isPalindrome(string: "abbc") == false)
assert(isPalindrome(string: "abcba") == true)
assert(isPalindrome(string: "abcbc") == false)


extension String {
    public subscript(_ i: Int) -> Character {
        return self[self.index(startIndex, offsetBy: i)]
    }
}

/// Find the longest palindrome in the given string
/// - Parameter string: string to find longest palindrome
/// - Returns: longest palindrome
///
/// Time complexity: O(n**2)
func longestPalindrome(string: String) -> String {
    var dp = [[Bool]](repeating: [Bool](repeating: false, count: string.count), count: string.count)
    for i in 0..<string.count {
        dp[i][i] = true
    }

    var start = 0
    var maxLength = 1

    for i in 0..<(string.count - 1) {
        if string[i] == string[i+1] {
            dp[i][i+1] = true
            start = i
            maxLength = 2
        }
    }

    for k in 2..<string.count {
        for i in 0..<(string.count-k) {
            let j = i + k
            if dp[i+1][j-1] && string[i] == string[j] {
                dp[i][j] = true
                if k + 1 > maxLength {
                    start = i
                    maxLength = k + 1
                }
            }
        }
    }

    return String(string.suffix(string.count - start).prefix(maxLength))
}

assert(longestPalindrome(string: "bcdcgga") == "cdc")
assert(longestPalindrome(string: "abcdcba") == "abcdcba")
assert(longestPalindrome(string: "abbabcde") == "abba")
assert(longestPalindrome(string: "abcdcef") == "cdc")
assert(longestPalindrome(string: "abbc") == "bb")
```

# Manacher's

O(n) algorithm

```swift
func modifiedString(string: String) -> [Character] {
    var ret = [Character](repeating: "#", count: 2 * string.count + 1)
    for i in 0..<string.count {
        ret[2 * i + 1] = string[i]
    }
    return ret
}

extension Array where Element == Character {
    subscript(safe index: Int) -> Character? {
        if 0 <= index && index < self.count {
            return self[index]
        }
        return nil
    }
}

/// Find the longest palindrome substring from the given string
/// - Parameter string: string to find longest palindome substring
/// - Returns: longest palindome substring
///
/// To find even length of palindrome substring, '#' Characters are added to the beginning, end, and between the Characters.
/// If there are more than one longest palindrome substring, returns the first one (with lower index)
///
/// Time complexity: O(n) (Manacher's algorithm)
func manacher(string: String) -> String {
    let str = modifiedString(string: string)
    var lps = [Int](repeating: 0, count: str.count)

    var r = -1
    var p = -1
    var lpsCenter = -1
    var maxRadius = -1
    for i in 0..<str.count {
        if r >= i {
            lps[i] = min(r - i, lps[2 * p - i])
        } else {
            lps[i] = 0
        }
        while let l = str[safe: i - lps[i] - 1], let r = str[safe: i + lps[i] + 1], l == r {
            lps[i] += 1
        }
        if i + lps[i] > r {
            r = i + lps[i]
            p = i
        }
        if lps[i] > maxRadius {
            maxRadius = lps[i]
            lpsCenter = i
        }
    }
    let e = str[lpsCenter-maxRadius...lpsCenter+maxRadius]
    return String(e.filter { $0 != "#" })
}

print(manacher(string: "bcdcgga"))
print(manacher(string: "abcdcba"))
print(manacher(string: "abbabcde"))
print(manacher(string: "abcdcef"))
print(manacher(string: "abbc"))
print(manacher(string: "abcd"))
```
