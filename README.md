<!doctype html>
<html lang="zh-Hant">
  <head>
    <meta charset="UTF-8" />
    <title>147測驗</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f0f4f8;
        margin: 0;
        padding: 40px;
        font-size: 1.6em;
      }
      #container {
        max-width: 1200px;
        margin: auto;
        background: #fff;
        padding: 40px;
        border-radius: 20px;
      }
      .hidden {
        display: none;
      }
      h1,
      h2 {
        text-align: center;
        color: #333;
        font-size: 2.4em;
      }
      #rules {
        background: #e9ecef;
        padding: 20px;
        margin-bottom: 40px;
        border-radius: 10px;
        font-size: 1.2em;
      }
      .btn {
        background: #007bff;
        color: #fff;
        border: none;
        padding: 20px 40px;
        margin: 10px;
        border-radius: 10px;
        cursor: pointer;
        font-size: 1.2em;
      }
      .btn:hover {
        background: #0056b3;
      }
      #leaveBtn {
        background: #dc3545;
      }
      #progress,
      #timer {
        font-weight: bold;
        font-size: 1.4em;
      }
      .question {
        margin: 40px 0 20px;
        font-size: 1.8em;
      }
      .options label {
        display: block;
        margin-bottom: 16px;
        font-size: 1.4em;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 40px;
        font-size: 1.2em;
      }
      th,
      td {
        border: 1px solid #ccc;
        padding: 16px;
        text-align: left;
      }
      .status img {
        width: 30px;
      }
    </style>
  </head>
  <body>
    <div id="container">
      <div id="welcome">
        <h1>147測驗</h1>
        <div id="rules">
          <p><strong>考試注意事項 / Exam Rules:</strong></p>
          <p>
            !!!建議使用電腦版網頁模式!!! /It is recommended to use the desktop
            web mode
          </p>

          <p>
            1. 請輸入姓名後才能開始作答。 / You must enter your name to start
            the quiz.
          </p>
          <p>
            2. 選擇答案後即視為作答完成，無法修改。 / Selecting an option will
            immediately submit your answer, no changes allowed.
          </p>
          <p>
            3. 考試限時80分鐘，自動倒數。 / The quiz is timed for 80 minutes,
            countdown starts immediately.
          </p>
          <p>
            4. 作答途中可隨時點擊「離開考試」提前結束。 / You can click "Leave
            Quiz" anytime to finish early.
          </p>
          <p>
            5. 完成後會自動顯示所有答題結果與成績。 / Results and scores will be
            displayed after completion.
          </p>
          <p>
            6. 答對題目顯示O，答錯題目顯示X。 / Correct answers will show O,
            incorrect answers will show X.
          </p>
        </div>
        <input
          type="text"
          id="nameInput"
          placeholder="輸入姓名 / Enter your name"
          style="
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            font-size: 1.4em;
          "
        />
        <button id="startBtn" class="btn">開始測驗 / Start Quiz</button>
      </div>

      <div id="quiz" class="hidden">
        <div>
          <span id="welcomeName"></span>
          <span id="timer" style="float: right">80:00</span>
        </div>
        <div id="progress">
          題數: <span id="current">0</span> / <span id="total">0</span>
        </div>
        <div class="question" id="questionText"></div>
        <div class="options" id="options"></div>
        <button id="prevBtn" class="btn" style="background: #6c757d">
          上一題 / Previous
        </button>

        <button id="leaveBtn" class="btn">離開考試 / Leave Quiz</button>
      </div>

      <div id="results" class="hidden">
        <h2>測驗結果 / Results</h2>
        <table>
          <thead>
            <tr>
              <th>題目</th>
              <th>您的答案</th>
              <th>正確答案</th>
              <th>結果</th>
            </tr>
          </thead>
          <tbody id="resultsBody"></tbody>
        </table>
        <div
          id="scoreSummary"
          style="text-align: center; margin-top: 20px; font-size: 1.2em"
        ></div>
      </div>
    </div>

    <script>
      document.getElementById("prevBtn").onclick = () => {
        if (current > 0) {
          current-- // 題目數減一
          answers.pop() // 把最後一次作答紀錄取消
          showQ() // 顯示上一題
        }
      }

      // (這裡放剛剛的完整questions陣列)
      const questions = [
        {
          question:
            "What kind of treatment can not produce certain internal properties of steel？",
          options: ["A. Abrasion", "B. Alloying", "C. Cold working"],
          answer: "A",
        },
        {
          question:
            "Which the following can not prevent metal fatigue happen？",
          options: [
            "A. Use appropriate tools",
            "B. Maintain drawing size and tolerance",
            "C. Keep sharp corners",
          ],
          answer: "C",
        },
        {
          question: "Bronze is an alloy of copper and",
          options: ["A. Beryllium", "B. Zinc", "C. Tin"],
          answer: "C",
        },
        {
          question:
            "Which following item is correct that describe the hardness tester (1)Brinell hardness tester uses a hardened spherical ball, which is forced into the surface of the metal. (2)Vickers hardness tester uses a 136 square pyramid indenter, which is forced into the surface of the sample.",
          options: [
            "A. Only (1) is correct",
            "B. Only (2) is correct",
            "C. Both (1) and (2) are correct",
          ],
          answer: "C",
        },
        {
          question:
            "Advanced composite materials possess superior specific stiffness. What is the definition of specific stiffness？",
          options: [
            "A. Elastic modulus divided by density",
            "B. Strength divided by density",
            "C. Elastic modulus multiplied by density",
          ],
          answer: "A",
        },
        {
          question:
            "If a self-locking nut (non-metallic insert) with a split-pin locking feature that is being shimmed for fit, how many times could it be re-torqued up to adjust the shim thickness for split pin fit?",
          options: ["A. 4 times", "B. 6 times", "C. 10 times"],
          answer: "C",
        },
        {
          question:
            "Holes for rivet shall be drilled straight and perpendicular (within _?_ ) to the surface against which the manufactured head will bear.",
          options: ["A. ±2°", "B. ±3°", "C. ±4°"],
          answer: "A",
        },
        {
          question: "The material letter code for 2017 aluminum alloy rivet is",
          options: ["A. D", "B. AD", "C. DD"],
          answer: "A",
        },
        {
          question:
            "In order to form the shop head and fill all clearance space in the hole, how many times of rivet diameter of monel rivet length shall exceed the material thickness before riveted?",
          options: ["A. 1", "B. 1.2", "C. 1.4"],
          answer: "A",
        },
        {
          question:
            "In order to achieve the proper pre-load for a given joint when installing flush head rivets, all provisions shall be made to achieve sufficient protrusion (no less than__?__) of each flush head rivet in the countersink prior to driving.",
          options: ["A. 0.000”", "B. 0.001”", "C. 0.002”"],
          answer: "B",
        },
        {
          question:
            "Which one is not the main factor while spring manufacturing?",
          options: [
            "A. Heat treatment",
            "B. Corrosion resistance",
            "C. Material selection",
          ],
          answer: "B",
        },
        {
          question: "In the graph, what is A?",
          options: [
            "A. Tension spring",
            "B. Compression spring",
            "C. Torsion spring",
          ],
          answer: "A",
        },
        {
          question: "Which factor is important for lower coils of spring?",
          options: [
            "A. Corrosion",
            "B. Unsymmetric load",
            "C. Temperature factor",
          ],
          answer: "B",
        },
        {
          question: "Which bearing can take the biggest thermal expansion?",
          options: [
            "A. Ball bearing",
            "B. Sliding bearing",
            "C. Cylindrical roller bearing",
          ],
          answer: "C",
        },
        {
          question: "Which bearing has the longest fatigue life?",
          options: [
            "A. Ball bearing",
            "B. Tapered roller bearing",
            "C. Sliding bearing",
          ],
          answer: "B",
        },
        {
          question: "Which one is not the advantage of anti-friction bearing?",
          options: [
            "A. Low friction",
            "B. Requires less lubricant",
            "C. High static strength",
          ],
          answer: "C",
        },
        {
          question:
            "Which coupling consists of an expansion coupling and an elastic coupling?",
          options: [
            "A. Safety coupling",
            "B. Solid coupling",
            "C. Movable coupling",
          ],
          answer: "C",
        },
        {
          question:
            "Which one of the following is the drive gear of the Planetary sun gear?",
          options: ["A. Ring gear", "B. Planetary gears", "C. Sun gear"],
          answer: "C",
        },
        {
          question:
            "Which transmission component only can transmit pulling forces?",
          options: ["A. Rotary selector", "B. Quadrant", "C. Cable"],
          answer: "C",
        },
        {
          question:
            "What is the maximum number of terminal fitting thread should show outside the barrel end of turnbuckle",
          options: ["A. 3 threads", "B. 5 threads", "C. 2 threads"],
          answer: "A",
        },
        {
          question:
            "In any over-torqued fastener assembly, both the bolt, screw or stud and the nut shall be...",
          options: ["A. Repaired", "B. Rejected", "C. Reworked"],
          answer: "B",
        },
        {
          question:
            "What correct type of material must be used in all fittings with lock-wire holes to prevent loosening of fitting?",
          options: ["A. Rope", "B. Bandage", "C. Lock-wire"],
          answer: "C",
        },
        {
          question:
            "Sloped shop heads are allowed, provided the mean height is greater than the appropriate minimum height dimensional requirement and the minimum height of the sloped shop head shall be greater than ___D, where D is the diameter of rivet.",
          options: ["A. 0.25D", "B. 0.35D", "C. 0.45D"],
          answer: "A",
        },
        {
          question:
            "How many times of rivet diameter of the nominal height of rivet should be riveted?",
          options: ["A. 0.5D", "B. 0.7D", "C. 0.9D"],
          answer: "A",
        },
        {
          question:
            "After shaved for flush, under no circumstances shall the rivet head be reduced by more than __?__ of the nominal head diameter.",
          options: ["A. 0.05D", "B. 0.1D", "C. 0.15D"],
          answer: "A",
        },
        {
          question:
            "What is the main cause for the difference of load-deformation chart between increasing or reducing the load on spring?",
          options: [
            "A. Mechanical energy",
            "B. Frictional energy",
            "C. Potential energy",
          ],
          answer: "B",
        },
        {
          question:
            "Bearings typically have to deal with two kinds of loads. What are the two loads?",
          options: [
            "A. Friction and thrust",
            "B. Radial and thrust",
            "C. Radial and centrifugal",
          ],
          answer: "B",
        },
        {
          question:
            "Which bearing allows minor angular displacements between rotating shaft and bearing housing?",
          options: [
            "A. Ball bearing",
            "B. Cylindrical roller bearing",
            "C. Spherical roller bearing",
          ],
          answer: "C",
        },
        {
          question:
            "If we talk about the structural elements of power transmission within control mechanisms, which of following requirements is NOT included?",
          options: ["A. Safety", "B. Lightweight", "C. Power source"],
          answer: "C",
        },
        {
          question:
            "The minimum clearance between control cables and adjacent cable systems and structure or component shall be",
          options: [
            'A. 1.00" near supports',
            'B. 0.25" near supports',
            'C. 0.50" near supports',
          ],
          answer: "C",
        },
        {
          question: "Select an extra-flexible cable from the list.",
          options: ["A. 1×7 cable", "B. 1×19 cable", "C. 7×19 cable"],
          answer: "C",
        },
        {
          question: "Wires with aluminum conductors are only used for",
          options: [
            "A. Large loads to save weight",
            "B. Fire protection to take higher temperature",
            "C. Passenger entertainment system to reduce voltage drop",
          ],
          answer: "A",
        },
        {
          question:
            "Two or more separate wires within the same insulation and protective sheath is referred to as a:",
          options: ["A. Screened wire", "B. Coaxial cable", "C. Cable"],
          answer: "C",
        },
        {
          question:
            "The number system of AWG (American Wire Gauge) is related to the wire",
          options: [
            "A. Combined resistance and current-carrying capacity",
            "B. Current-carrying capacity",
            "C. Cross-sectional area",
          ],
          answer: "C",
        },
        {
          question:
            "For Boeing wire number W104-2444-22-R or Airbus wire number 2322-0121R, this letter R is…:",
          options: [
            "A. Color code",
            "B. Designation code",
            "C. Manufacturer code",
          ],
          answer: "A",
        },
        {
          question:
            "Installation of self-sealing nuts against slopes greater than ? degrees is not recommended.",
          options: ["A. 0.5°", "B. 1°", "C. 2°"],
          answer: "C",
        },
        {
          question:
            "For self-locking inserts, the bolts shall not be re-used for that purpose more than",
          options: ["A. 8 times", "B. 10 times", "C. 16 times"],
          answer: "B",
        },
        {
          question: "What is the function of teeth shape spring washer?",
          options: [
            "A. Prevent nut loosening",
            "B. Bolt protection",
            "C. Bolt adjustment",
          ],
          answer: "A",
        },
        {
          question:
            "Which one can obtain the non-linear characteristic spring?",
          options: [
            "A. Higher pitch angle of spring",
            "B. Lower pitch angle of spring",
            "C. Unequal pitch of spring",
          ],
          answer: "C",
        },
        {
          question:
            "The effective coils number of spring is the total number of coils minus the number of end coil. Is the above description true?",
          options: ["A. Yes", "B. No", "C. Not related"],
          answer: "A",
        },
        {
          question:
            "Which bearing from the list has the lowest starting resistance?",
          options: [
            "A. Roller bearing",
            "B. Sleeve bearing",
            "C. Ball bearing",
          ],
          answer: "C",
        },
        {
          question: "Attention must be paid to ____ when removing bearing.",
          options: ["A. Cleaning", "B. Humidity", "C. Tool access"],
          answer: "A",
        },
        {
          question: "Which type bearing is not an anti-friction bearing?",
          options: [
            "A. Ball bearing",
            "B. Tapered roller bearing",
            "C. Slot bearing",
          ],
          answer: "C",
        },
        {
          question:
            "What is the mechanical advantage of the machine with effort 90 lb, effort arm 60 inches; resistance 300 lb, resistance arm 18 inches?",
          options: ["A. 3.33", "B. 0.3", "C. 1"],
          answer: "A",
        },
        {
          question:
            "The minimum clearance between control cables and adjacent cable systems and structure or component shall be",
          options: [
            "A 1.00〞 near supports",
            " B 0.25〞 near supports",
            "C 0.50〞 near supports.",
          ],
          answer: "C",
        },
        {
          question:
            "All cable assemblies manufactured with swaged terminals shall be proof loaded to",
          options: [
            "A. 60% of ultimate cable strength",
            "B. 80% of ultimate cable strength",
            "C. 40% of ultimate cable strength",
          ],
          answer: "A",
        },
        {
          question:
            "What is the circular-mil dimension of a wire 0.050 in diameter?",
          options: ["A. 50", "B. 2500", "C. 250"],
          answer: "B",
        },
        {
          question:
            "Particular attention should be given for the mechanical strength and installation handling of wires when using wire sizes smaller than",
          options: ["A. AWG 00", "B. AWG 02", "C. AWG 20"],
          answer: "C",
        },
        {
          question:
            "The most common method of attaching a pin or socket to an individual wire in an electrical connector is by",
          options: ["A. Crimping", "B. Soldering", "C. Crimping and soldering"],
          answer: "A",
        },
        {
          question:
            "In general the shielded wires are used in systems to protect against",
          options: ["A. Fire", "B. Fluid", "C. Noise"],
          answer: "C",
        },
        {
          question:
            "An aircraft maintenance licence for aeroplanes and helicopters has the following categories:",
          options: [
            "A. Category A, B1, B2, C",
            "B. Category A, B, C",
            "C. Category A, B, C, D",
            "D. Category A1, A2, B1, B2",
          ],
          answer: "A",
        },
        {
          question:
            "In accordance with article 114 of the Civil Aviation Act, when the repair or alteration of aviation products, appliances and parts has neither been inspected in accordance with the pertinent manual nor been signed and released in appropriate form or tag by qualified personnel, the repair station may receive what kind of punishments？",
          options: [
            "A. Warning or fine NT$10,000-30,000",
            "B. Warning or fine NT$6,000-300,000",
            "C. Warning or fine NT$60,000-100,000",
            "D. Warning or fine NT$60,000-300,000",
          ],
          answer: "D",
        },
        {
          question:
            "In accordance with the Aircraft Flight Operation Regulations, how many years of maintenance experience of transport category aircraft is required for a Director of Maintenance？",
          options: ["A. 6 months", "B. 1 year", "C. 2 years", "D. 5 years"],
          answer: "D",
        },
        {
          question:
            "One aircraft that is certificated as transport category by CAA or by civil aviation authority of the State of Design and prohibited for acrobatic operation is categorized as:",
          options: [
            "A. Commuter Category Aircraft",
            "B. Transport Category Aircraft",
            "C. Normal Category Aircraft",
            "D. Utility Category Aircraft",
          ],
          answer: "B",
        },
        {
          question:
            "In accordance with article 103 of the Civil Aviation Act, operating an uncertified aircraft may result in:",
          options: [
            "A. 1 year in prison or fine",
            "B. 5 years in prison or fine",
            "C. 10 years in prison or fine",
            "D. 20 years in prison or fine",
          ],
          answer: "B",
        },

        {
          question: "An aircraft should not be refueled when:",
          options: [
            "A. the APU is running..",
            "B. navigation and landing light in operation.",
            "C. within 10 meters (30 feet) of radar operating..",
          ],
          answer: "C",
        },
        {
          question:
            "Additives that speed up a paint's cure time are referred to as",
          options: ["A. hardeners.", "B. retarders.", "C. lacquer. "],
          answer: "A",
        },
        {
          question:
            "What is the torque value of a nut to use a 6 in torque wrench setting 150 in-lb with a 3 in extension bar",
          options: ["A. 205 in-lb.", "B. 215 in-lb.", "C. 225 in-lb"],
          answer: "C",
        },
        {
          question:
            "What type of meter is used for measuring very high values of resistance？",
          options: [
            "A. Mega-ohmmeter.",
            "B. Shunt-type ohmmeter.",
            "C. Multimeter. ",
          ],
          answer: "A",
        },
        {
          question:
            "The Air Transport Association of America (ATA) Specification No.100 (1) establishes a standard for the presentation of technical data in maintenance manuals. (2) divides the air craft into numbered systems and subsystems in order to simplify locating maintenance instructions. Regarding the above statements,",
          options: [
            "A. neither No.1 nor No.2 is true.",
            "B. both No.1 and No.2 are true.",
            "C. only No.1 is true.",
          ],
          answer: "B",
        },
        {
          question: "The width of a visible outline on a drawing is______.",
          options: ["A. 0.3 mm.", "B. 0.7 mm.", "C. 0.5 mm ."],
          answer: "B",
        },
        {
          question:
            "A drawing in which all of the parts are brought together as an assembly is called",
          options: [
            "A.  a sectional drawing.",
            "B. a detail drawing.",
            "C. an installation drawing.",
          ],
          answer: "C",
        },
        {
          question:
            "What is the allowable manufacturing tolerance for a bushing where the outside dimensions shown on the blueprint are: 1.0625 + 0.0025 - 0.0003 ?",
          options: ["A. 0.0028.", "B. 1.0650.", "C. 1.0647 "],
          answer: "A",
        },
        {
          question: "What does GA stand for on a drawing?",
          options: [
            "A. General Assembly.",
            "B. General Arrangement.",
            "C. Gradient Axis.",
          ],
          answer: "B",
        },
        {
          question: "What is the maximum bow allowed in a strut?",
          options: ["A. 1 in 200..", "B. 1 in 500.", "C. 1 in 600. "],
          answer: "C",
        },
        {
          question:
            "Which is the process of obtaining the necessary electrical conductivity between the component metallic parts of the aircraft?",
          options: [
            "A. grounding test",
            "B. insulation test",
            "C. bonding test .",
          ],
          answer: "C",
        },
        {
          question:
            "A repair has a double riveted joint. The shear strength would be_____.",
          options: ["A.  125%", "B.  75%.", "C. 100%."],
          answer: "B",
        },
        {
          question:
            "The minimum acceptable dimensions, where D equals the diameter of the rivet shank, for a formed end of a rivet are",
          options: [
            "A. height 0.65D, width 1.0D",
            "B. height 0.5D, width 1.5D",
            "C. height 0.65D, width 1.5D",
          ],
          answer: "B",
        },
        {
          question:
            "A scratch or nick in aluminum alloy tubing can be repaired by burnishing, provided the scratch or nick does not ",
          options: [
            "A. appear in the heel of a bend in the tube.",
            "B. exceed 5 percent of the tube diameter.",
            "C. exceed 10 percent of the tube diameter. ",
          ],
          answer: "A",
        },
        {
          question:
            "What is the complete designation for an aluminum nut for a flared fitting that will fit a 5/8 inch OD line?",
          options: ["A. AN818 D 5", "B. AN818 D 10", "C. AN818 D 16"],
          answer: "B",
        },
        {
          question: "What is the length of a spring if NOT under load?",
          options: ["A. free length.", "B. block length", "C. wire length "],
          answer: "A",
        },
        {
          question: "Thrust bearings transmit",
          options: [
            "A.  thrust loads, thus limiting axial movement..",
            "B.  radial loads, thus limiting axial movement..",
            "C. thrust loads, thus limiting radial movement. ",
          ],
          answer: "A",
        },
        {
          question: "Backlash is a type of wear associated with",
          options: ["A. gears.", "B. rivets.", "C. bearings."],
          answer: "A",
        },
        {
          question:
            "On large aircraft, the ____ of the immediate area should be taken into consideration when using a tension meter?",
          options: ["A. Temperature..", "B. Moisture.", "C. Altitude."],
          answer: "A",
        },
        {
          question:
            "The sharpest bend that can be placed in a piece of metal without critically weakening the part is called the",
          options: [
            "A. maximum radius of bend.",
            "B. minimum radius of bend.",
            "C.  bend allowance.",
          ],
          answer: "B",
        },

        {
          question:
            "When using a megger to test insulation resistance, capacitive filters should be disconnected for what reason?",
          options: [
            "A.  Remove the risk of damage to the megger.",
            "B.  Remove the spurious readings caused by the capacitors charging and discharging.",
            "C.  Prevent damage to the filters.",
          ],
          answer: "C",
        },
        {
          question:
            "What type of diagram is used to explain a principle of operation, rather than show the parts as they actually appear?",
          options: [
            "A. A block diagram.",
            "B. A pictorial diagram.",
            "C.  A system schematic diagram.",
          ],
          answer: "C",
        },
        {
          question: "A line used to show an edge that is not visible is a",
          options: ["A.  phantom line.", "B. break line.", "C.  hidden line."],
          answer: "C",
        },
        {
          question:
            "If there is a positive allowance between the smallest possible hole and the largest possible shaft, the fit is known as",
          options: [
            "A. a transition fit.",
            "B. a clearance fit.",
            "C. an interference fit.",
          ],
          answer: "B",
        },
        {
          question:
            "The length of a blended repair of corrosion should be no less than:",
          options: [
            "A. 10 times its depth.",
            "B.  20 times its depth.",
            "C. 5 times its depth.",
          ],
          answer: "B",
        },
        {
          question:
            "When coaxial cable is installed, it should be secured firmly along its entire length …",
          options: [
            "A. at 1-ft intervals.",
            "B. wherever the cable sags.",
            "C. at 2-ft intervals.",
          ],
          answer: "A",
        },
        {
          question:
            "Many hoses are made of TFE or Teflon and may be used for practically all __? encountered on an airplane.",
          options: ["A.  fuel.", "B. hydraulic fluid.", "C. fluids ."],
          answer: "C",
        },
        {
          question:
            "What are the coils in a coil spring that move or deflect under a load?",
          options: ["A. coil surge.", "B. end coils.", "C. active coils."],
          answer: "C",
        },
        {
          question: "A tapered roller bearing is designed to take:",
          options: [
            "A. radial loads only.",
            "B. both radial and axial loads.",
            "C.  axial loads only.",
          ],
          answer: "B",
        },
        {
          question: "Control chains should be fitted in an aircraft",
          options: [
            "A. with the minimum of slack in the chain.",
            "B.  so that the chain can be removed easily.",
            "C. with as much slack as possible to allow for contraction.",
          ],
          answer: "A",
        },
        {
          question:
            "The completed terminal sleeves should be checked periodically with the proper gauge called",
          options: ["A. Go-no-go gauge.", "B. Surface gage.", "C. Depth gage."],
          answer: "A",
        },
        {
          question: "The skin on an aircraft is normally manufactured from",
          options: [
            "A. 2024 aluminum alloy.",
            "B. 7075 aluminum alloy.",
            "C. 2117 aluminum alloy.",
          ],
          answer: "A",
        },
        {
          question: "In bending, the neutral axis is:",
          options: [
            "A.  a line created by the points where the compressive load of the bend equals the tension load.",
            "B.  specified on the inside radius on modem aircraft drawings.",
            "C.  the same as the setback.",
          ],
          answer: "A",
        },
        {
          question:
            "The approved maintenance organization must retain maintenance records and associated maintenance data to aircraft and aircraft component for at least:",
          options: ["A. 1 year.", "B. 2 years.", "C. 5 years."],
          answer: "B",
        },
        {
          question:
            "Page Blocks are used in the Maintenance Manual to enable the user to locate the desired information more rapidly. Information for Removal and Installation can be found in page…",
          options: ["A.  201 to 300", "B.  301 to 400.", "C. 401 to 500."],
          answer: "C",
        },
        {
          question:
            "A pressure gauge is fitted to a Dead Weight Tester. The piston area is 0.25 sq.in. and the total mass of the mass carrier and masses is 5 LB. If the pressure gauge is accurate, what pressure in pounds per square inch (PSI) will it read?:",
          options: ["A. 1.25 psi.", "B. 20 psi.", "C. 200 psi."],
          answer: "B",
        },
        {
          question: "Which statement relating to electrical wiring is true?",
          options: [
            "A. When attaching a terminal to the end of an electric cable, it should be determined that the strength of the cable to terminal joint is at least twice the tensile strength of the cable.",
            "B. When attaching a terminal to the end of an electric cable, it should be determined that the strength of the cable to terminal joint is at least equal to the tensile strength of the cable itself.",
            "C. All electric cable splices should be covered with soft insulating tubing (spaghetti) for mechanical protection against external abrasion.",
          ],
          answer: "B",
        },
        {
          question:
            "WWhat is the complete designation for a 90 elbow to join two flared 3/8 in OD tubes?",
          options: ["A. AN821–3.", "B. AN821–6.", "C. AN821-9."],
          answer: "B",
        },
        {
          question: "After cleaning a bearing, it should be",
          options: [
            "A. left in free air to dry naturally.",
            "B. dried with clean, warm, dry compressed air",
            "C. dried with lint free rags.",
          ],
          answer: "B",
        },
        {
          question: "How do you check a chain for elongation?:",
          options: [
            "A. Hang chain up, check sight line and measure.",
            "B. Adjust the end fittings.",
            "C. Lay flat on a table, apply tensile load and measure.",
          ],
          answer: "C",
        },
        {
          question:
            "The angle of pulley and pulley alignment line should not over",
          options: ["A. 1 degree.", "B. 2 degree.", "C. 3 degree."],
          answer: "B",
        },
        {
          question:
            "What is used to protect a cable routed through a small bulkhead hole? ",
          options: ["A. seal.", "B. grommet.", "C. compound."],
          answer: "B",
        },
        {
          question:
            "When drilling stainless steel, the drill used should have an included angle of",
          options: [
            "A. 90 degree and turn at a low speed.",
            "B. 90 degree and turn at a high speed.",
            "C. 140 degree and turn at a low speed",
          ],
          answer: "C",
        },
        {
          question: "Prior to aluminum alloy bonding we use.",
          options: ["A. alkaline etch", "B. acid etch", "C. solvent wipe"],
          answer: "B",
        },
        {
          question:
            "A mechanic has completed a bonded honeycomb repair using the potted compound repair technique. What non-destructive testing method is used to determine the soundness of the repair after the repair has cured?:",
          options: [
            "A. Eddy current test.",
            "B. Metallic ring test.",
            "C. Ultrasonic test.",
          ],
          answer: "B",
        },
        {
          question: "When towing an aircraft:",
          options: [
            "A. all nosewheel aircraft must be towed backwards.",
            "B. if the aircraft has a steerable nose wheel, the locking scissors should be set to full swivel.",
            "C. all struts should be fully deflated.",
          ],
          answer: "B",
        },
        {
          question:
            "The aircraft is airworthy as: (1) Parts and materials come from any organization. (2) All the scheduled maintenance tasks are done in schedule. (3) the aircraft is confirmed  to conform the Type Certificate",
          options: [
            "A. Only (1) and (2) are required.",
            "B. Only (1) and (3) are required.",
            "C. Only (2) and (3) are required.",
          ],
          answer: "C",
        },
        {
          question: "When testing a fuel metering unit, how is it checked?:",
          options: [
            "A. With the meter in series with the unit.",
            "B. With the unit disconnected.",
            "C. With the meter in parallel with the unit.",
          ],
          answer: "A",
        },
        {
          question:
            "Use the triangular files are limited to internal angles less than",
          options: ["A 120 degree", "B 100 degree", "C 90 degree"],
          answer: "C",
        },
        {
          question: "2 micron meters is",
          options: ["A 0.002inch ", "B 0.002mm", "C 0.0002inch"],
          answer: "B",
        },
      ]

     // 接下來的控制邏輯
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[array[i], array[j]] = [array[j], array[i]]
  }
  return array
}

let shuffledQuestions = shuffle([...questions])
let current = 0,
  total = shuffledQuestions.length,
  timer = 80 * 60,
  interval,
  answers = []

document.getElementById("total").innerText = total

function fmtTime(s) {
  const m = Math.floor(s / 60)
    .toString()
    .padStart(2, "0")
  const sec = (s % 60).toString().padStart(2, "0")
  return m + ":" + sec
}

document.getElementById("startBtn").onclick = () => {
  const n = document.getElementById("nameInput").value.trim()
  if (!n) return alert("請輸入姓名 / Enter your name")
  document.getElementById("welcome").classList.add("hidden")
  document.getElementById("quiz").classList.remove("hidden")
  document.getElementById("welcomeName").innerText = "歡迎: " + n
  interval = setInterval(() => {
    timer--
    document.getElementById("timer").innerText = fmtTime(timer)
    if (timer <= 0) finish()
  }, 1000)
  showQ()
}

document.getElementById("leaveBtn").onclick = finish

// 新增上一題功能
document.getElementById("prevBtn").onclick = () => {
  if (current > 0) {
    current--;
    answers.pop();
    showQ();
  }
}

function showQ() {
  if (current >= total) return finish()
  document.getElementById("current").innerText = current + 1
  const q = shuffledQuestions[current]
  document.getElementById("questionText").innerText = q.question
  const optDiv = document.getElementById("options")
  optDiv.innerHTML = ""

  // 把選項包成 {text, isAnswer} 格式
  let optionsWithFlag = q.options.map((option) => ({
    text: option,
    isAnswer: option.charAt(0) === q.answer,
  }))

  // 打亂選項
  for (let i = optionsWithFlag.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[optionsWithFlag[i], optionsWithFlag[j]] = [
      optionsWithFlag[j],
      optionsWithFlag[i],
    ]
  }

  // 顯示打亂後的選項
  optionsWithFlag.forEach((opt) => {
    const lbl = document.createElement("label")
    const rd = document.createElement("input")
    rd.type = "radio"
    rd.name = "opt"
    rd.value = opt.text  // 注意這裡記錄的是完整文字
    rd.onchange = () => {
      answers.push({ 
        q, 
        selectedText: opt.text, 
        correctText: q.options.find(optItem => optItem.charAt(0) === q.answer),
        correct: opt.isAnswer 
      })
      current++
      showQ()
    }
    lbl.append(rd, " ", opt.text)
    optDiv.append(lbl)
  })
}

function finish() {
  clearInterval(interval)
  document.getElementById("quiz").classList.add("hidden")
  const tb = document.getElementById("resultsBody")
  tb.innerHTML = ""
  let correct = 0
  answers.forEach((a) => {
    const tr = document.createElement("tr")
    tr.innerHTML = `
      <td>${a.q.question}</td>
      <td>${a.selectedText}</td>
      <td>${a.correctText}</td>
      <td>${a.correct ? "O" : "X"}</td>
    `;
    if (a.correct) correct++
    tb.append(tr)
  })

  document.getElementById("scoreSummary").innerText =
    `答對 ${correct} 題 / 已作答 ${answers.length} 題 / 共 ${total} 題`
  document.getElementById("results").classList.remove("hidden")
}


    </script>
  </body>
</html>
