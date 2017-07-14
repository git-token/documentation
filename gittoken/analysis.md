---
references:
- id: git-scm2017
  author:
    - family: Git, et. al.
  title: gitworkflows - An overview of recommended workflows with Git
  container-title: 'https://git-scm.com/docs/gitworkflows'
  accessed:
    year: 2017
    month: 7
    day: 11
---
@import "https://cdn.plot.ly/plotly-latest.min.js"

### Evaluation of Open Source Projects

```{javascript id:"chj51b6znn", element:"<div id='test' style='width:600px;height:250px;margin-bottom:300px'></div>", id:"chj51b78o5"}
TESTER = document.getElementById('test')
Plotly.plot(TESTER, [{
  x: [38438497236, 18401040701, 1376206596, 235578951, 228864420, 87398581, 56423927],
  y: [14409, 8680, 8299, 3958, 258, 1492, 706],
  name: 'Contributions',
  text: ['Bitcoin', 'Ethereum', 'Ethereum Classic', 'Golem', 'Gnosis', 'Status', 'Aragon'],
  textposition: 'top center',
  marker: {
    size: [38.43, 18.40, 1.37, .23, .22, .08, .05]
  },
  mode: 'markers+text'
}],{
  height: 600,
  margin: { t: 100, l: 50 },
  title: 'Git Commits Vs. Market Capitalization'
});


```

GitToken maps the total supply of the token to contributions made to the project.
