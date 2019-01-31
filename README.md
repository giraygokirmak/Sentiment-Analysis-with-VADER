# Sentiment-Analysis-with-VADER
Sentiment Analysis on any language with just 13 lines of code
 </br>

<div id="notebook" class="js-html">
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="ch">#!python3 -m pip install --user vaderSentiment nltk feedparser googletrans</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[6]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">feedparser</span>
<span class="kn">import</span> <span class="nn">nltk</span>
<span class="kn">from</span> <span class="nn">nltk.sentiment.vader</span> <span class="k">import</span> <span class="n">SentimentIntensityAnalyzer</span>
<span class="kn">from</span> <span class="nn">googletrans</span> <span class="k">import</span> <span class="n">Translator</span> <span class="c1"># Currently vader lexicon is only trained in English so we trust google for NMT</span>

<span class="n">nltk</span><span class="o">.</span><span class="n">download</span><span class="p">(</span><span class="s1">'vader_lexicon'</span><span class="p">)</span> <span class="c1"># Download pretrained vader lexicon</span>

<span class="n">translator</span> <span class="o">=</span> <span class="n">Translator</span><span class="p">()</span>
<span class="n">sid</span> <span class="o">=</span> <span class="n">SentimentIntensityAnalyzer</span><span class="p">()</span>

<span class="n">NewsFeed</span> <span class="o">=</span> <span class="n">feedparser</span><span class="o">.</span><span class="n">parse</span><span class="p">(</span><span class="s2">"https://tr.investing.com/rss/news_25.rss"</span><span class="p">)</span> <span class="c1"># Investing.com Stock Exchange News RSS Feed</span>

<span class="n">df_feed</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">columns</span><span class="o">=</span><span class="p">[</span><span class="s1">'title'</span><span class="p">])</span>

<span class="k">for</span> <span class="n">entry</span> <span class="ow">in</span> <span class="n">NewsFeed</span><span class="o">.</span><span class="n">entries</span><span class="p">:</span>
    <span class="n">translated</span> <span class="o">=</span> <span class="n">translator</span><span class="o">.</span><span class="n">translate</span><span class="p">(</span><span class="n">entry</span><span class="p">[</span><span class="s1">'title'</span><span class="p">],</span> <span class="n">dest</span><span class="o">=</span><span class="s1">'en'</span><span class="p">)</span>
    <span class="n">df_feed</span> <span class="o">=</span> <span class="n">df_feed</span><span class="o">.</span><span class="n">append</span><span class="p">({</span><span class="s1">'title'</span><span class="p">:</span> <span class="n">entry</span><span class="p">[</span><span class="s1">'title'</span><span class="p">],</span>
                              <span class="s1">'compound'</span><span class="p">:</span> <span class="n">sid</span><span class="o">.</span><span class="n">polarity_scores</span><span class="p">(</span><span class="n">translated</span><span class="o">.</span><span class="n">text</span><span class="o">.</span><span class="n">lower</span><span class="p">())[</span><span class="s1">'compound'</span><span class="p">]},</span> <span class="n">ignore_index</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[5]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df_feed</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">50</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt output_prompt">Out[5]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>title</th>
      <th>compound</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Japonya piyasaları kapanışta yükseldi; Nikkei ...</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>YENİLEME 1-Türk Telekom 2018'de TL'deki değer ...</td>
      <td>-0.1280</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Çin piyasaları kapanışta yükseldi; Shanghai Co...</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VİOP-BIST-30 endeksinin Şubat sonu vadeli kont...</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eldorado Gold Kışladağ Altın Madeni'nde madenc...</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Avustralya piyasaları kapanışta düştü; S&amp;P/ASX...</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Türk Telekom 2018'de TL'deki değer kaybının da...</td>
      <td>-0.3182</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Arçelik'in 2018 yılı net kârı %1 artışla 851.8...</td>
      <td>0.6124</td>
    </tr>
    <tr>
      <th>8</th>
      <td>SOCAR EWE Turkey Holding ve grup şirketlerini ...</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Meksika piyasaları kapanışta yükseldi; S&amp;P/BMV...</td>
      <td>0.2960</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
 

</div>
