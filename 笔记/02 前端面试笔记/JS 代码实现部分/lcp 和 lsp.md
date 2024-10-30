
```js
function lcs(str1, str2) {

  const nA = str1.length + 1

  const nB = str2.length + 1

  

  const dp = Array.from({ length: nA }, () => new Array(nB).fill(0))

  

  // 初始化dp 子串 dp[i][0] dp[0][i] 肯定是 0

  for (let i = 0; i < nA; i++) {

    dp[i][0] = ''

  }

  for (let i = 0; i < nB; i++) {

    dp[0][i] = ''

  }

  

  for (let i = 1; i < nA; i++) {

    for (let j = 1; j < nB; j++) {

      if (str1[i - 1] === str2[j - 1]) {

        dp[i][j] = dp[i - 1][j - 1] + str1[i - 1]

      } else {

        dp[i][j] = dp[i - 1][j].length > dp[i][j - 1].length ? dp[i - 1][j] : dp[i][j - 1]

      }

    }

  }

  return dp[nA - 1][nB - 1]

}

console.log(lcs('1a2b3c4d5e', 'a1b2c3d4e5'))

console.log(lcs('abcde', 'abdce'))


```


```js
  

function lps(str1) {

  const str = str1

  // const str = '#' + str1.split('').join('#') + '#'

  const nA = str.length

  console.log(str)

  
  

  const dp = Array.from({ length: nA }, () => new Array(nA).fill(''))

  

  // 初始化dp 子串 dp[i][0] dp[0][i] 肯定是 0

  for (let i = 0; i < nA; i++) {

    dp[i][i] = str[i]

  }

  

  for (let i = nA - 1; i >= 0; i--) {

    for (let j = i + 1; j < nA; j++) {

      if (str[i] === str[j]) {

        if (i !== j) {

          dp[i][j] = str[i] + dp[i + 1][j - 1] + str[j]

        } else if (i + 1 === j) {

          dp[i][j] = str[i] + str[j]

        }

      } else {

        dp[i][j] = dp[i + 1][j].length > dp[i][j - 1].length ? dp[i + 1][j] : dp[i][j - 1]

      }

    }

  }

  

  for (let i = 0; i < nA; i++) {

    console.log(dp[i])

  }

  return dp[0][nA - 1]

}

  

console.log('lps', lps('awsdffdgsa'))
```