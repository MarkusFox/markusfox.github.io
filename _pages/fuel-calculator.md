---
title: "Sim Racing Fuel Calculator"
permalink: /fuel-calculator/
author_profile: false
# layout: empty
# toc: true
---
<!-- # Hello World -->
Hello fellow Sim-Racer! On this page you find a simple but effective tool to calculate your fuel consumption before or during the race.

<html>
  <head>
    <title>Fuel Calculation</title>
    <!-- <link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700" rel="stylesheet">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.5.0/css/all.css" integrity="sha384-B4dIYHKNBt8Bc12p+WXckhzcICo0wtJAoU8YZTY5qE0Id1GSseTk6S+L3BlXeVIU" crossorigin="anonymous"> -->
    <style>
      /* .testbox {
      display: flex;
      justify-content: center;
      align-items: center;
      height: inherit;
      padding: 20px;
      } */
      .colums {
      display:flex;
      justify-content:space-between;
      flex-direction:row;
      flex-wrap:wrap;
      align-items: flex-end;
      }
      .colums div {
      width:48%;
      }
      .times {
      display:flex;
      justify-content:space-between;
      flex-direction:row;
      flex-wrap:wrap;
      }
      .times input {
      width:32%;
      }
        .radio-toolbar {
        margin: 30px;
        }

        .radio-toolbar input[type="radio"] {
        opacity: 0;
        position: fixed;
        width: 0;
        }

        .radio-toolbar label {
            display: inline-block;
            width: 125px;
            color: #eaeaea;
            background-color: #252a34;
            padding: 10px 20px;
            font-family: sans-serif, Arial;
            font-size: 18px;
            border: 2px solid #444;
            border-radius: 4px;
        }

        .radio-toolbar label:hover {
        background-color: #699ea0;
        }

        .radio-toolbar input[type="radio"]:checked + label {
            background-color: #8cd2d5;
            <!-- border-color: #4c4; -->
        }

        .cent {
            width: 258px;
            margin: 0 auto;
        }

        #lapstotal, #timeleft {
            flex: 0 0 50%;
        }
    </style>
  </head>
  <body>
    <div class="testbox">
      <form action="/" name="myform">
        <!-- <div class="banner">
          <h1>Fuel Calculation</h1>
        </div> -->
        <div class="radio-toolbar">
            <div class="cent">
                <input id="choosetime" type="radio" name="select" onclick="swap();" checked/>
                <label for="choosetime">Minutes</label>
                <input id="chooselaps" type="radio" name="select" onclick="swap();" value="Laps"/>
                <label for="chooselaps">Laps</label>
            </div>
        </div>
        <div class="item">
            <label for="duration" id="durationlabel">Race length (in minutes)</label>
            <input id="duration" type="number" name="duration" onchange="calculate()"/>
        </div>
        <div class="times">
            <div class="item">
            <label for="laptime">Lap time</label>
            <input id="tmin" type="number" name="tmin" placeholder="1" onchange="calculate();"/>
            <input id="tsec" type="number" name="tsec" placeholder="28" onchange="calculate();"/>
            <input id="tms" type="number" name="tms" placeholder="500" onchange="calculate();"/>
            </div>
        </div>
        <div class="item">
          <label for="lapfuel">Fuel/Lap</label>
          <input id="lapfuel" type="text" name="lapfuel" placeholder="e.g. 2.48" onchange="calculate();"/>
        </div>
        <div class="colums">
            <div class="item">
            <label for="lapstotal">Total laps</label>
            <input id="lapstotal" type="text" name="lapstotal" value="0" readonly="readonly"/>
            </div>
            <div class="item">
            <label for="timeleft">Time left (when starting last lap)</label>
            <input id="timeleft" type="text" name="timeleft" value="-" readonly="readonly"/>
            </div>
        </div>
        <div class="results">
            <label for="fueltotal">Total fuel</label>
            <input id="fueltotal" type="text" name="fueltotal" value="0" readonly="readonly"/>
            <label for="outlaptotal">Incl. full outlap</label>
            <input id="outlaptotal" type="text" name="outlaptotal" value="0" readonly="readonly"/>
        </div>
      </form>
    </div>
    <script type="text/javascript">
        function updateValues(lapstotal, timeleft, fueltotal, outlaptotal) {
            document.myform.lapstotal.value = lapstotal.toFixed(0);
            document.myform.timeleft.value = timeleft;
            document.myform.fueltotal.value = fueltotal.toFixed(2);
            document.myform.outlaptotal.value = outlaptotal.toFixed(2);
        }
        function swap() {
            if (document.myform.choosetime.checked) {
                document.getElementById("durationlabel").innerHTML = "Race length (in minutes)";
            } else {
                document.getElementById("durationlabel").innerHTML = "Number of laps";
            }
        }
        function calculate() {
            
            var duration = parseFloat(document.myform.duration.value);
            var tmin = parseFloat(document.myform.tmin.value);
            var tsec = parseFloat(document.myform.tsec.value);
            var tms = parseFloat(document.myform.tms.value);
            var lapfuel = parseFloat(document.myform.lapfuel.value.replace(',', '.'));
            var arr = [duration, tmin, tsec, lapfuel]
            for (i=0; i < arr.length; i++) {
                if (Number.isNaN(arr[i])) {
                    updateValues(0, "-", 0, 0);
                    return;
                }
            }

            var totallaps = 0;
            var timeleft = "-";
            if (document.myform.choosetime.checked) {
                var minutes = duration;
                var raceseconds = minutes * 60;
                var lapseconds = tmin * 60 + tsec + parseFloat("0."+tms.toString());
                var lapratio = raceseconds / lapseconds;
                var secondsleft = Math.round(lapseconds * (lapratio - Math.floor(lapratio)));
                var minutesleft = Math.floor(secondsleft / 60);

                timeleft = minutesleft.toString() + "m " + (secondsleft % 60).toString() + "s";
                totallaps = Math.ceil(lapratio);
            } else {
                totallaps = duration;
            }
            
            updateValues(totallaps, timeleft, totallaps*lapfuel, totallaps*lapfuel+lapfuel);
        }
    </script>
  </body>
</html>