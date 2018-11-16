---


---

<h1 id="heroes-of-bresenham’s-might-and-chebychev’s-magic-lightning">Heroes of Bresenham’s might and Chebychev’s magic (Lightning)</h1>
<p>Вы попали в особенную трехмерную галактику. В ней свои правила и своя физика - Чебышевская, а лучи и пути - Брезенхемовские.<br>
Вам предстоит много смотреть в визуализатор, чтобы разобраться в принципе ее работы. В целом все как в реальном мире.</p>
<h1 id="правила-турнира">Правила турнира</h1>
<p>После сабмита вашего кода, он сыграет со всеми соперниками по 3 раза. Исходя из исхода игр вы займете место в турнирной таблице.<br>
В конце турнира система будет заморожена, а результаты пересчитаны. Каждый с каждым сыграют по 7 раз.<br>
Выигрыват та команда, которая после переигровки окажется на 1 месте.</p>
<h1 id="основные-понятия">Основные понятия</h1>
<h2 id="игровое-поле">Игровое поле</h2>
<p>Игровое поле представляет из себя дискретный куб с размерами 30x30x30, координаты в котором задаются тройками целых, неотрицательных чисел (x, y, z), где<br>
0 ≤ x &lt; 30, 0 ≤ y &lt; 30, 0 ≤ z &lt; 30.</p>
<h2 id="вектор">Вектор</h2>
<p>Тройка целых чисел v = (x, y, z).</p>
<h2 id="метрика-чебышёва">Метрика Чебышёва</h2>
<p>Если есть вектор v = (x, y, z), то clen(v) =  max { |x|, |y|, |z| } - метрика Чебышёва</p>
<h2 id="манхэттенская-метрика">Манхэттенская метрика</h2>
<p>Если есть вектор v = (x, y, z), то mlen(v) = |x| + |y| + |z| - манхэттенская метрика</p>
<h2 id="метод-построение-лучей">Метод построение лучей</h2>
<p>Все лучи строятся с помощью алгоритма Брезенхема.</p>
<h1 id="механика-взаимодействия">Механика взаимодействия</h1>
<ul>
<li>Состояние игры отправляется игрокам одновременно</li>
<li>Каждому игроку дается равное количество времени на ход</li>
<li>Команды игроков применяются одновременно</li>
</ul>
<h2 id="протокол-взаимодействия-с-ботом">Протокол взаимодействия с ботом</h2>
<p>Взаимодействие с ботом происходит через сообщения. Сервер отправляет сообщения боту через stdin процесса, а принимает сообщения от бота через stdout. Каждое сообщение - это строка в формате JSON, непосредственно за которой следует символ переноса строки ‘\n’. Больше нигде в строке символ ‘\n’ встречаться не может, иначе это будет воспринято как несколько сообщений! Длина сообщения не должна превышать 2048 символов.</p>
<p>Если отравить невалидный json, то засчитывается поражение!</p>
<h1 id="правила-игры">Правила игры</h1>
<p>Обе команды имеют флот, который состоит из кораблей.</p>
<h2 id="корабль">Корабль</h2>
<p>Корабль - это куб размером 2x2x2. Каждый корабль комплектуется определенным количеством оборудования.</p>
<h2 id="оборудование">Оборудование</h2>
<ul>
<li>Существует 4 типа оборудования: блок энергии, блок здоровья, двигатель, оружие.</li>
<li>У каждого оборудования есть свое уникальное название.</li>
</ul>
<h3 id="блок-здоровья">Блок здоровья</h3>
<ul>
<li>Изначальное количество здоровья зависит от соответствующей характеристики блока</li>
<li>Корабль жив, пока здоровье больше 0</li>
</ul>
<h3 id="блок-энергии">Блок энергии</h3>
<ul>
<li>Изначальное количество энергии зависит от соответствующей характеристики блока</li>
<li>Каждый ход количество энергии увеличивается, пока не достигнет максимума</li>
<li>Энергия используется для питания других блоков</li>
</ul>
<h3 id="двигатель">Двигатель</h3>
<ul>
<li>С помощью двигателя корабль может менять свою скорость</li>
<li>Если вылететь за границы карты, то корабль перестанет быть управляемым и улетит (т.е. погибнет)</li>
</ul>
<h3 id="оружие">Оружие</h3>
<ul>
<li>Бластер стреляет по цели из ближайшей (по манхэттенской метрике) точки корабля</li>
<li>Луч выходит из корабля и идет в направлении цели пока не выполнится одно из условий:
<ul>
<li>попадет в корабль</li>
<li>длина луча будет больше или равна радиусу (по чебышевской метрике)</li>
<li>достигнет границ поля</li>
</ul>
</li>
<li>Луч строится по алгоритму Брезенхема</li>
<li>Если хотя бы одна точка луча касается корабля, то у него отнимается здоровье в зависимости от мощности удара оружия</li>
</ul>
<h2 id="команды">Команды</h2>
<h3 id="move">Move</h3>
<ul>
<li>Автопилот, который подвинет корабль ближе к цели</li>
<li>Не умеет обходить препятствия</li>
<li>Максимальное ускорение = 1 (по чебышевской метрике)</li>
<li>Максимальная скорость = 1 (по чебышевской метрике)</li>
<li>Невозможно вылететь за границы поля</li>
</ul>
<h3 id="accelerate">Accelerate</h3>
<ul>
<li>Более гибкое управление движением</li>
<li>Добавляет вектор к текущему вектору скорости</li>
<li>Из-за неосторожности можно вылететь за границы</li>
</ul>
<h3 id="attack">Attack</h3>
<ul>
<li>Атакует оружием.</li>
</ul>
<h1 id="процесс-игры">Процесс игры</h1>
<p>Игра делится на 2 фазы: draft, battle.</p>
<h2 id="фаза-draft">Фаза Draft</h2>
<ul>
<li>На ответ дается 10 секунд, в противном случае засчитывается поражение.</li>
<li>На данном этапе эта фаза нужна только для дополнительного времени</li>
</ul>
<h3 id="input">Input</h3>
<p>Пустой json.</p>
<p>Пример:</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span><span class="token punctuation">}</span>
</code></pre>
<h3 id="output">Output</h3>
<p>Пустой json.</p>
<p>Пример:</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span><span class="token punctuation">}</span>
</code></pre>
<h2 id="фаза-battle">Фаза Battle</h2>
<p>В начале у каждого игрока 5 кораблей типа Scout (подробнее про него ниже). Они расставлены симметрично в разных углах поля. У них нет скорости.</p>
<ul>
<li>Каждый шаг игры сервер присылает игрокам состояние</li>
<li>Игроки отвечают набором команд</li>
<li>На ответ дается 1 секунда, в противном случае засчитывается поражение</li>
<li>Порядок действий каждый ход:
<ul>
<li>Накопление энергии</li>
<li>Движение (MOVE и ACCELERATE)</li>
<li>Атака</li>
</ul>
</li>
<li>При столкновении с другим коралем отнимается одна единица здоровья</li>
</ul>
<h3 id="input-1">Input</h3>
<p>На вход игроку приходит json-объект (State), в котором сначала описываются его корабли, потом корабли соперника, потом следы эффектов, которые применялись ходом ранее.<br>
Про свои корабли известно больше информации.</p>
<h3 id="state">State</h3>
<ul>
<li>My - <code>list of my Ships</code></li>
<li>Opponent - <code>list of opponent Ships</code></li>
<li>FireInfos - <code>list of FireInfos</code></li>
</ul>
<h3 id="ship">Ship</h3>
<ul>
<li>Id - <code>integer</code></li>
<li>Health - <code>integer</code></li>
<li>Energy - <code>integer</code> (неизвестно для оппонента)</li>
<li>Position - <code>vector</code></li>
<li>Velocity - <code>vector</code></li>
<li>Equipment - <code>list of Block</code> (неизвестно для оппонента)</li>
</ul>
<h3 id="blocks">Blocks</h3>
<ul>
<li>Name - <code>string</code></li>
<li>Type - <code>integer</code> (0 - energy block, 1 - blaster block, 2 - engine block, 3 - health block)</li>
</ul>
<p>остальный характеристики блока добавляются в зависимости от типа</p>
<h4 id="energy-block">Energy block</h4>
<ul>
<li>IncrementPerTurn - <code>integer</code></li>
<li>MaxEnergy - <code>integer</code></li>
<li>StartEnergy - <code>integer</code></li>
</ul>
<h4 id="gun-block">Gun block</h4>
<ul>
<li>Damage - <code>integer</code>,</li>
<li>EnergyPrice - <code>integer</code>,</li>
<li>Radius - <code>integer</code>,</li>
<li>EffectType - <code>integer</code> (всегда 0)</li>
</ul>
<h4 id="engine-block">Engine block</h4>
<ul>
<li>MaxAccelerate - <code>integer</code></li>
</ul>
<h4 id="health-block">Health block</h4>
<ul>
<li>MaxHealth - <code>integer</code></li>
<li>StartHealth - <code>integer</code></li>
</ul>
<h3 id="fireinfo">FireInfo</h3>
<ul>
<li>Source - <code>vector</code></li>
<li>Target - <code>vector</code></li>
<li>EffectType - <code>integer</code> (всегда 0)</li>
</ul>
<p>Пример State:</p>
<ul>
<li>В примере описан корабль Scout.</li>
</ul>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
    <span class="token string">"My"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token string">"Id"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
            <span class="token string">"Velocity"</span><span class="token punctuation">:</span> <span class="token string">"0/0/0"</span><span class="token punctuation">,</span>
            <span class="token string">"Position"</span><span class="token punctuation">:</span> <span class="token string">"0/0/0"</span><span class="token punctuation">,</span>
            <span class="token string">"Energy"</span><span class="token punctuation">:</span> <span class="token number">50</span><span class="token punctuation">,</span>
            <span class="token string">"Health"</span><span class="token punctuation">:</span> <span class="token number">100</span><span class="token punctuation">,</span>
            <span class="token string">"Equipment"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
                <span class="token punctuation">{</span>
                    <span class="token string">"Type"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
                    <span class="token string">"IncrementPerTurn"</span><span class="token punctuation">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
                    <span class="token string">"MaxEnergy"</span><span class="token punctuation">:</span> <span class="token number">100</span><span class="token punctuation">,</span>
                    <span class="token string">"StartEnergy"</span><span class="token punctuation">:</span> <span class="token number">50</span><span class="token punctuation">,</span>
                    <span class="token string">"Name"</span><span class="token punctuation">:</span> <span class="token string">"big_energy"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token string">"Type"</span><span class="token punctuation">:</span> <span class="token number">3</span><span class="token punctuation">,</span>
                    <span class="token string">"MaxHealth"</span><span class="token punctuation">:</span> <span class="token number">100</span><span class="token punctuation">,</span>
                    <span class="token string">"StartHealth"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
                    <span class="token string">"Name"</span><span class="token punctuation">:</span> <span class="token string">"big_health"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token string">"Type"</span><span class="token punctuation">:</span> <span class="token number">2</span><span class="token punctuation">,</span>
                    <span class="token string">"MaxAccelerate"</span><span class="token punctuation">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
                    <span class="token string">"Name"</span><span class="token punctuation">:</span> <span class="token string">"big_engine"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token string">"Type"</span><span class="token punctuation">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
                    <span class="token string">"Damage"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
                    <span class="token string">"EnergyPrice"</span><span class="token punctuation">:</span> <span class="token number">10</span><span class="token punctuation">,</span>
                    <span class="token string">"Radius"</span><span class="token punctuation">:</span> <span class="token number">5</span><span class="token punctuation">,</span>
                    <span class="token string">"EffectType"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
                    <span class="token string">"Name"</span><span class="token punctuation">:</span> <span class="token string">"big_blaster"</span>
                <span class="token punctuation">}</span>
            <span class="token punctuation">]</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token string">"Opponent"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token string">"Id"</span><span class="token punctuation">:</span> <span class="token number">10000</span><span class="token punctuation">,</span>
            <span class="token string">"Velocity"</span><span class="token punctuation">:</span> <span class="token string">"0/0/0"</span><span class="token punctuation">,</span>
            <span class="token string">"Position"</span><span class="token punctuation">:</span> <span class="token string">"0/0/25"</span><span class="token punctuation">,</span>
            <span class="token string">"Health"</span><span class="token punctuation">:</span> <span class="token number">100</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token string">"FireInfos"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token string">"Source"</span><span class="token punctuation">:</span> <span class="token string">"0/4/25"</span><span class="token punctuation">,</span>
            <span class="token string">"Target"</span><span class="token punctuation">:</span> <span class="token string">"0/3/20"</span><span class="token punctuation">,</span>
            <span class="token string">"EffectType"</span><span class="token punctuation">:</span> <span class="token number">0</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="output-1">Output</h3>
<h3 id="useroutput">UserOutput</h3>
<ul>
<li>UserCommands - <code>list of Command</code></li>
<li>Message - <code>string</code></li>
</ul>
<h3 id="command">Command</h3>
<ul>
<li>Command - <code>string</code> (MOVE, ACCELERATE, ATTACK)</li>
<li>Parameters - зависит от значения Command</li>
</ul>
<h4 id="move-parameters">Move parameters</h4>
<ul>
<li>Id - <code>integer</code> (id корабля)</li>
<li>Target - <code>vector</code></li>
</ul>
<h4 id="accelerate-parameters">Accelerate parameters</h4>
<ul>
<li>Id - <code>integer</code> (id корабля)</li>
<li>Vector - <code>vector</code></li>
</ul>
<h4 id="attack-parameters">Attack parameters</h4>
<ul>
<li>Id - <code>integer</code> (id атакующего корабля)</li>
<li>Name - <code>string</code> (название оружия)</li>
<li>Target - <code>vector</code></li>
</ul>
<p>Пример UserOutput:</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
    <span class="token string">"UserCommands"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token string">"Command"</span><span class="token punctuation">:</span> <span class="token string">"MOVE"</span><span class="token punctuation">,</span>
            <span class="token string">"Parameters"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
                <span class="token string">"Id"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
                <span class="token string">"Target"</span><span class="token punctuation">:</span> <span class="token string">"21/22/18"</span>
            <span class="token punctuation">}</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token string">"Command"</span><span class="token punctuation">:</span> <span class="token string">"ATTACK"</span><span class="token punctuation">,</span>
            <span class="token string">"Parameters"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
                <span class="token string">"Id"</span><span class="token punctuation">:</span> <span class="token number">1</span><span class="token punctuation">,</span>
                <span class="token string">"Name"</span><span class="token punctuation">:</span> <span class="token string">"big_blaster"</span><span class="token punctuation">,</span>
                <span class="token string">"Target"</span><span class="token punctuation">:</span> <span class="token string">"0/2/27"</span>
            <span class="token punctuation">}</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token string">"Message"</span><span class="token punctuation">:</span> <span class="token string">"my_message"</span>
<span class="token punctuation">}</span>
</code></pre>
<h1 id="как-определяется-исход-игры">Как определяется исход игры</h1>
<ul>
<li>Если к концу хода один из соперников потерял свой флот, то победа присуждается другому.</li>
<li>В случае одновременной потери флота присуждается ничья.</li>
<li>Если к концу 1000 (TODO возможно надо уменьшить см. Constants.MaxTurns) хода игра не закончилась присуждается ничья.</li>
</ul>
<h1 id="системные-ограничения">Системные ограничения</h1>
<ul>
<li>Размер исходного файла не должен превышать 1 Мб</li>
<li>Ограничения на ресурсы во время исполнения программы
<ul>
<li>одно ядро</li>
<li>1024 Mb commit size</li>
<li>1024 Mb physical memory</li>
</ul>
</li>
</ul>
<h1 id="программные-ограничения">Программные ограничения</h1>
<ul>
<li>Запрещена работа с файловой системой и с сетью</li>
<li>Запрещено запускать процессы из своего кода</li>
</ul>
<p>Любая попытка обойти ограничения, сломать или хакнуть систему приведет к дисквалификации команды.</p>
<h1 id="поддерживаемые-языки">Поддерживаемые языки</h1>
<ul>
<li>C# 7.3 (.NET Freamework 4.7.2)
<ul>
<li>AllowUnsafe = true</li>
<li>OverflowChecks = false</li>
<li>Newtonsoft.Json 11.0.2</li>
<li>C5 2.5.3</li>
<li>MoreLinq 3.0.0</li>
</ul>
</li>
<li>Python 3.7.0 (Anaconda3 v5.3.0)</li>
<li>JavaScript (node.js v10.13.0)</li>
<li>1C (OScript v1.0.21.1)</li>
</ul>

