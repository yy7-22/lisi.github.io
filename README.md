<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>15题精简版MBTI人格测评</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:"微软雅黑",sans-serif;}
        body{background:#f5f7fa;padding:20px;max-width:800px;margin:0 auto;}
        .box{background:#fff;border-radius:10px;padding:30px;box-shadow:0 2px 10px #ddd;}
        h1{text-align:center;color:#333;margin-bottom:20px;font-size:22px;}
        .tip{color:#666;text-align:center;margin-bottom:30px;line-height:1.6;}
        .q-item{margin:20px 0;padding-bottom:15px;border-bottom:1px solid #eee;}
        .q-title{font-size:16px;color:#222;margin-bottom:12px;line-height:1.6;}
        .opt{margin:8px 0;cursor:pointer;}
        button{display:block;width:100%;height:45px;background:#409eff;color:#fff;border:none;border-radius:6px;font-size:16px;margin-top:30px;cursor:pointer;}
        button:hover{background:#337ecc;}
        .result{margin-top:30px;padding:20px;background:#f0f7ff;border-radius:8px;display:none;}
        .res-title{font-size:20px;font-weight:bold;margin-bottom:15px;color:#2c5282;}
        .res-desc{line-height:1.8;color:#333;}
    </style>
</head>
<body>
    <div class="box">
        <h1>15题精简版MBTI人格测评</h1>
        <p class="tip">请根据自身真实情况选择，答案无对错，完成后自动生成测评结果</p >

        <div id="questionBox"></div>
        <button onclick="getResult()">提交查看结果</button>

        <div class="result" id="resultBox">
            <div class="res-title" id="resType"></div>
            <div class="res-desc" id="resContent"></div>
        </div>
    </div>

<script>
const questions = [
    {id:1,text:"周末休息时，和朋友聚会热闹一天，比独自在家待着更能让你恢复精力",dim:"EI"},
    {id:2,text:"面对陌生的社交场合，你通常会主动上前和陌生人搭话、融入话题",dim:"EI"},
    {id:3,text:"遇到烦心事时，你更愿意找朋友倾诉交流，而不是自己一个人消化情绪",dim:"EI"},
    {id:4,text:"做小组作业时，你更习惯当组织者带动氛围，而不是默默完成自己的部分",dim:"EI"},
    {id:5,text:"看故事/电影时，你更关注剧情里真实发生的细节，而不是背后的隐喻和深层含义",dim:"SN"},
    {id:6,text:"制定计划时，你更习惯按已有的经验和步骤执行，而不是尝试全新的方法",dim:"SN"},
    {id:7,text:"别人给你讲一件事时，你更在意对方说的具体事实，而不是话里的潜在意思",dim:"SN"},
    {id:8,text:"畅想未来时，你更偏向于规划具体的目标和步骤，而不是做天马行空的想象",dim:"SN"},
    {id:9,text:"朋友找你吐槽烦心事时，你第一反应是帮他分析问题、给出解决方案，而不是先共情情绪",dim:"TF"},
    {id:10,text:"做决策时，你更看重事情的公平性和逻辑对错，而不是身边人的感受和关系",dim:"TF"},
    {id:11,text:"被别人指出错误时，你更在意自己的问题有没有被纠正，而不是对方说话的态度",dim:"TF"},
    {id:12,text:"面对截止日期的任务，你会提前做好计划、按时完成，而不是拖到最后一刻才赶工",dim:"JP"},
    {id:13,text:"你的房间/书桌大多时候是整齐有序的，东西都有固定的摆放位置",dim:"JP"},
    {id:14,text:"出门旅行时，你更习惯提前做好行程规划，而不是到了地方再随机决定去哪",dim:"JP"},
    {id:15,text:"你更喜欢稳定、有规律的生活，而不是充满变化和未知的日常",dim:"JP"}
];
const options = ["完全不符合","不太符合","中立","比较符合","完全符合"];

function renderQ(){
    let html = "";
    questions.forEach((q,idx)=>{
        html += `<div class="q-item">
            <div class="q-title">${idx+1}. ${q.text}</div>`;
        options.forEach((opt,i)=>{
            html += `<div class="opt">
                <input type="radio" name="q${q.id}" value="${i+1}"> ${opt}
            </div>`;
        });
        html += `</div>`;
    });
    document.getElementById("questionBox").innerHTML = html;
}

function getResult(){
    let ei=0,sn=0,tf=0,jp=0;
    questions.forEach(q=>{
        let val = document.querySelector(`input[name="q${q.id}"]:checked`);
        if(!val){alert("请完成所有题目再提交！");return;}
        let num = parseInt(val.value);
        if(q.dim === "EI") ei += num;
        if(q.dim === "SN") sn += num;
        if(q.dim === "TF") tf += num;
        if(q.dim === "JP") jp += num;
    });

    let e_i = ei >= 12 ? "E" : "I";
    let s_n = sn >= 12 ? "S" : "N";
    let t_f = tf >= 9 ? "T" : "F";
    let j_p = jp >= 12 ? "J" : "P";
    let type = e_i + s_n + t_f + j_p;

    const resMap = {
        ISTJ:{tag:"沉稳实干家",desc:"严谨负责、重视规则、注重细节，做事踏实靠谱，是团队的稳定担当。建议偶尔跳出固有经验，尝试新方式，多关注他人情绪。"},
        ISFJ:{tag:"温柔守护者",desc:"体贴细心、乐于助人，习惯默默付出，重视承诺与人际关系。建议多留独处空间，学会适度拒绝，减少内耗。"},
        INFJ:{tag:"深刻引路人",desc:"洞察力强、富有理想、共情出众，追求事物的内在意义。建议不要过度消耗精力，把大目标拆解为小行动，避免空想。"},
        INTJ:{tag:"睿智战略家",desc:"逻辑缜密、目标感强、独立理性，擅长长远规划。建议多倾听不同意见，接纳计划外的变化，不必事事追求完美。"},
        ISTP:{tag:"灵活手艺人",desc:"务实理性、动手能力强、适应力佳，擅长处理突发问题，不喜拘束。建议主动表达想法，学着制定长期规划。"},
        ISFP:{tag:"自由艺术家",desc:"温和敏感、热爱生活、审美出色，不喜冲突，习惯用行动传递善意。建议设立小目标，避免过于随性，主动表达自身需求。"},
        INFP:{tag:"理想治愈者",desc:"内心柔软、创造力强、共情力高，坚守自我价值观。建议走出自我世界，把想法落地执行。"},
        INTP:{tag:"好奇思考者",desc:"逻辑清晰、求知欲旺盛，热爱钻研、思维跳脱。建议把思考转化为行动，把控时间，按时完成任务。"},
        ESTP:{tag:"活力挑战者",desc:"外向灵活、行动力强、善于应变，喜爱新鲜事物，社交能力突出。建议做好长远规划，兼顾他人感受。"},
        ESFP:{tag:"快乐表演者",desc:"热情开朗、感染力强，是人群中的开心果，热爱当下生活。建议培养长期坚持的习惯，合理安排独处时间。"},
        ENFP:{tag:"热情追梦人",desc:"脑洞丰富、充满活力、擅长鼓舞他人，创造力十足。建议聚焦目标，提升专注力，不要同时开展过多事情。"},
        ENTP:{tag:"聪明辩论家",desc:"思维敏捷、口才出众、乐于创新，喜欢探索新可能。建议多倾听他人观点，将创意落地为实际成果。"},
        ESTJ:{tag:"果断管理者",desc:"执行力强、有领导力、重视效率，行事有原则、作风利落。建议放缓姿态，照顾团队情绪，包容不同做事风格。"},
        ESFJ:{tag:"热心东道主",desc:"友善热情、重视人际、擅长照顾他人，人缘极佳。建议优先顾及自身需求，不必一味迎合他人。"},
        ENFJ:{tag:"魅力教育家",desc:"共情力强、富有号召力、责任感重，善于引导他人。建议减少过度操心，专注自我成长，避免精力透支。"},
        ENTJ:{tag:"远见指挥官",desc:
