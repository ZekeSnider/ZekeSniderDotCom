---
layout: page
title: Alpha Test
permalink: /alpha-test/
image: AlphaMale.jpg
---

<script type="text/javascript">
var pointValues = [
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[-1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[1, 0],
	[0, 1, 2, 3],
	[0, 1, 2],
	[2, 1, 0],
	[3, 2, 1],
	[0, 1, 2, 3],
	[0, 1, 4, 3]
]

var gradingRubric = {
	"0": {
		"title": "the runt",
		"img": "Runt.jpg",
		"description": "You are definitely the weakest of the group. You try to compensate for it in other ways but it doesn't always work out. It's hard to live with this truth, but you wake up every day and realize it. Stay cucked."
	},
	"50": {
		"title": "the Meek follower",
		"img": "william.png",
		"description": "You're not exceptionally strong, or exceptionally weak. You follow the pack and do your doondy. Don't stick your neck out too far for risk of the alpha or m-m-maybe later beta trying to score some cool points off you. Stuck between a runt and a hard place."
	},
	"60": {
		"title": "the B-Beta (you'll be alpha maybe later)",
		"img": "beta.jpg",
		"description": "You want to be alpha, but you're not quite there yet. It's hard being 2nd place, but you can deal. You can dethrone the alpha, maybe later. You're not ready for that level of power yet. Hone your skills and try again later. "
	},
	"85": {
		"title": "Mega Alpha AF",
		"img": "alphaf.jpg",
		"description": "You are undoubtedly extremely alpha. Everyone is frightened by your presence. You can strong arm your way into whatever you like, just because of your strong aura."
	}
}

function getMaxScore() {
	var maxScore = 0;
	for (var i=0; i<pointValues.length; i++) {
	    maxScore += Math.max.apply(null, pointValues[i]);
	}
	return maxScore;
}

function getResult(score) {
	var maxScore = getMaxScore();
	var percentageScore = (score / maxScore) * (100);
	var myResult;
	var currentHigh = -10;
	for (var key in gradingRubric) {
		var thisCutoff = parseInt(key);
	    if (percentageScore >= thisCutoff && thisCutoff > currentHigh) {
	    	myResult = gradingRubric[thisCutoff];
	    	currentHigh = thisCutoff;
	    }
	}
	return myResult;
}

function getRadioValue(number) {
	var radios = document.getElementsByName(number.toString());

	for (var i = 0; i < radios.length; i++) {
	    if (radios[i].checked) {
	        // do whatever you want with the checked radio
	        var value = pointValues[number][radios[i].value];
	        if (value === undefined) {
	        	return 0;
	        }
	        else {

	        	return value;
	        }
	    }
	}
	return 0;
}

function countScore() {
	var pointTotal = 0;
	for (var i=1; i<pointValues.length; i++) {
	    pointTotal += getRadioValue(i);
	}
	return pointTotal;
}

function updateDOMFromResult(result) {
	document.getElementById('testResults').style.display = "block";
	document.getElementById("testResultImg").src = "/assets/" + result["img"];
	document.getElementById("testResultTitle").innerHTML = 'You are ' + result["title"];
	document.getElementById("testResultCaption").innerHTML = result["description"];
	
}
function submitTest() {
	updateDOMFromResult(getResult(countScore()));
	
}
</script>
*How alpha are you?* This test will finally deliver the definitive answer to that question. These questions have been scientifically crafted to determine with absolute certainty how alpha you are, from timid cuck, to alpha af dominant male. Answer each question carefully.


<img src="/assets/Bubble.jpg">

1. Do you have the alpha app installed?<br>
<input type="radio" name="1" value="0">Yes<br>
<input type="radio" name="1" value="1">No<br>

1. Do you always know which way North is?  
<input type="radio" name="2" value="0">Yes<br>
<input type="radio" name="2" value="1">No<br>

1. Do you "forget" to shave, "forget" to bring a razor, then bring up your well rounded scruff?  
<input type="radio" name="3" value="0">Yes<br>
<input type="radio" name="3" value="1">No<br>

1. Would you go to a shooting range?  
<input type="radio" name="4" value="0">Yes<br>
<input type="radio" name="4" value="1">No<br>

1. Do you carry a knife?  
<input type="radio" name="5" value="0">Yes<br>
<input type="radio" name="5" value="1">No<br>

1. Do you lift regularly?  
<input type="radio" name="6" value="0">Yes<br>
<input type="radio" name="6" value="1">No<br>

1. I make people afraid when I take off my clothes.   
<input type="radio" name="7" value="0">Yes<br>
<input type="radio" name="7" value="1">No<br>

1. Do you like getting your ears rubbed?  
<input type="radio" name="8" value="0">Yes<br>
<input type="radio" name="8" value="1">No<br>

1. Can you skip rocks well?  
<input type="radio" name="9" value="0">Yes<br>
<input type="radio" name="9" value="1">No<br>

1. Can you swim well?  
<input type="radio" name="10" value="0">Yes<br>
<input type="radio" name="10" value="1">No<br>

1. Are you good at billiards?  
<input type="radio" name="11" value="0">Yes<br>
<input type="radio" name="11" value="1">No<br>

1. Do you conceal carry?  
<input type="radio" name="12" value="0">Yes<br>
<input type="radio" name="12" value="1">No<br>

1. Are you a boy genius??  
<input type="radio" name="13" value="0">Yes<br>
<input type="radio" name="13" value="1">No<br>

1. How often on average do you masturbate?  
<input type="radio" name="14" value="0">NEVER<br>
<input type="radio" name="14" value="1">1-3 times per week<br>
<input type="radio" name="14" value="2">Once per day<br>
<input type="radio" name="14" value="3">Twice or more per day<br>

1. How often do you throw up?  
<input type="radio" name="15" value="0">Rarely when sick / never<br>
<input type="radio" name="15" value="1">Moderately often (every few months)<br>
<input type="radio" name="15" value="2">Extremely often<br>

1. How much do you chew food?  
<input type="radio" name="16" value="0">1 gulp down the hatch and then I ask for other people's leftovers<br>
<input type="radio" name="16" value="1">6 bites even number<br>
<input type="radio" name="16" value="2">Slow graze like a cow, always the nice guy that finishes last<br>

1. How fast can you do a quickie?  
<input type="radio" name="17" value="0">< 30 seconds<br>
<input type="radio" name="17" value="1">≈ 2 minutes<br>
<input type="radio" name="17" value="2">≥ 5 minutes<br>

1. What is your spice tolerance?  
<input type="radio" name="18" value="0">Cuck tier<br>
<input type="radio" name="18" value="1">🔥Moderate level<br>
<input type="radio" name="18" value="2">🔥🔥Level up level<br>
<input type="radio" name="18" value="3">🔥🔥🔥Extremely spicy good boy level<br>

1. Do you spit brah?  
<input type="radio" name="19" value="0">Yeah on the ground<br>
<input type="radio" name="19" value="1">Out the window of a moving car<br>
<input type="radio" name="19" value="2">During football to assert my dominance<br>
<input type="radio" name="19" value="3">No, I swallow.<br>

<input type="submit" value="Submit" onclick="submitTest()">

<div id="testResults" display="none">
	<h2 id="testResultTitle"></h2>
	<img id="testResultImg">
	<p id="testResultCaption"></p>
</div>

*This test is under distinguished copyright © {{ 'now' | date: "%Y" }} Zach & Emily*




