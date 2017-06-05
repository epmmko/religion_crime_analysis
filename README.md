<div tabindex="-1" id="notebook" class="border-box-sizing">

<div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

# Project information[¶](#Project-information)

## Data set regions and data description: US and Ann Arbor, MI[¶](#Data-set-regions-and-data-description:-US-and-Ann-Arbor,-MI)

1) USA 2010 Religion data for each metropolitan area (including Ann Arbor area)

2) USA 2010 FBI crime data for each metropolitan area (including Ann Arbor area)

## Question statement[¶](#Question-statement)

1) Based on the number of religion adherents, did the religion help to reduce the crime, statistically?

2) How did Ann Arbor, MI compared to the rest of the US in terms of religions adherents and crime/murder cases?

## Link to the data sets[¶](#Link-to-the-data-sets)

1) 2010 Religion data

    - http://www.thearda.com/Archive/Files/Downloads/RCMSMT10_DL2.asp
    - Data dictionary
    - http://www.thearda.com/Archive/Files/Codebooks/RCMSMT10_CB.asp

2) 2010 FBI crime data

    - https://ucr.fbi.gov/crime-in-the-u.s/2010/crime-in-the-u.s.-2010/tables/table-6

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Data cleaning[¶](#Data-cleaning)

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [1]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">matplotlib</span> <span class="k">as</span> <span class="nn">mpl</span>
<span class="kn">import</span> <span class="nn">re</span>
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [2]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="c1">#cleaning religion data</span>
<span class="n">df1</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_excel</span><span class="p">(</span><span class="s2">r".\ProjData\U.S. Religion Census.XLSX"</span><span class="p">)</span>
<span class="n">df1</span> <span class="o">=</span> <span class="n">df1</span><span class="o">.</span><span class="n">loc</span><span class="p">[:,[</span><span class="s1">'MTNAME'</span><span class="p">,</span><span class="s1">'TOTADH'</span><span class="p">,</span><span class="s1">'POP2010'</span><span class="p">]]</span>
<span class="n">df1</span> <span class="o">=</span> <span class="n">df1</span><span class="o">.</span><span class="n">dropna</span><span class="p">(</span><span class="n">axis</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">df1</span><span class="p">[</span><span class="s1">'ADH per capita'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df1</span><span class="p">[</span><span class="s1">'TOTADH'</span><span class="p">]</span> <span class="o">/</span> <span class="n">df1</span><span class="p">[</span><span class="s1">'POP2010'</span><span class="p">]</span>
<span class="n">df1</span> <span class="o">=</span> <span class="n">df1</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="s1">'MTNAME'</span><span class="p">)</span>
<span class="n">df1</span><span class="p">[</span><span class="s1">'Metro name'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df1</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">x</span><span class="o">.</span><span class="n">name</span><span class="p">[</span><span class="mi">0</span><span class="p">:</span><span class="o">-</span><span class="mi">30</span><span class="p">],</span> <span class="n">axis</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span>
<span class="n">df1</span> <span class="o">=</span> <span class="n">df1</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="s1">'Metro name'</span><span class="p">)</span>
<span class="n">df1</span> <span class="o">=</span> <span class="n">df1</span><span class="p">[[</span><span class="s1">'ADH per capita'</span><span class="p">]]</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [3]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="c1">#cleaning 2010 FBI data</span>
<span class="n">df2</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_excel</span><span class="p">(</span><span class="s2">r".\ProjData\table-6.xls"</span><span class="p">,</span> <span class="n">skiprows</span> <span class="o">=</span> 
                    <span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="mi">2224</span><span class="p">,</span> <span class="mi">2225</span><span class="p">,</span> <span class="mi">2226</span><span class="p">,</span> <span class="mi">2227</span><span class="p">,</span> <span class="mi">2228</span><span class="p">,</span> <span class="mi">2229</span><span class="p">])</span>
<span class="n">df2</span><span class="p">[</span><span class="s1">'Metropolitan Statistical Area'</span><span class="p">]</span>  <span class="o">=</span> <span class="n">df2</span><span class="p">[</span><span class="s1">'Metropolitan Statistical Area'</span><span class="p">]</span><span class="o">.</span><span class="n">ffill</span><span class="p">()</span>
<span class="n">df2</span> <span class="o">=</span> <span class="n">df2</span><span class="o">.</span><span class="n">loc</span><span class="p">[:,[</span><span class="s1">'Metropolitan Statistical Area'</span><span class="p">,</span><span class="s1">'Population'</span><span class="p">,</span><span class="s1">'Violent crime'</span><span class="p">,</span><span class="s1">'Property crime'</span><span class="p">,</span>
                <span class="s1">'Murder and nonnegligent manslaughter'</span><span class="p">]]</span>
<span class="n">df2</span><span class="p">[</span><span class="n">df2</span><span class="p">[:]</span> <span class="o">==</span> <span class="s1">' '</span><span class="p">]</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">nan</span>
<span class="n">df2</span><span class="p">[</span><span class="s1">'Total crime'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df2</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">np</span><span class="o">.</span><span class="n">nansum</span><span class="p">([</span><span class="n">x</span><span class="p">[</span><span class="s1">'Violent crime'</span><span class="p">],</span><span class="n">x</span><span class="p">[</span><span class="s1">'Property crime'</span><span class="p">]]),</span> <span class="n">axis</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span>

<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'Metropolitan Statistical Area'</span><span class="p">])</span><span class="o">.</span><span class="n">ffill</span><span class="p">()</span>
<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2_g</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'Metropolitan Statistical Area'</span><span class="p">])</span><span class="o">.</span><span class="n">bfill</span><span class="p">()</span>
<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2_g</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'Metropolitan Statistical Area'</span><span class="p">])</span><span class="o">.</span><span class="n">max</span><span class="p">()</span>
<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2_g</span><span class="o">.</span><span class="n">drop</span><span class="p">(</span><span class="n">df2_g</span><span class="o">.</span><span class="n">index</span><span class="p">[</span><span class="mi">0</span><span class="p">],</span><span class="n">axis</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2_g</span><span class="o">.</span><span class="n">rename</span><span class="p">(</span><span class="n">columns</span><span class="o">=</span><span class="p">{</span><span class="n">df2_g</span><span class="o">.</span><span class="n">columns</span><span class="p">[</span><span class="mi">3</span><span class="p">]:</span><span class="s1">'Murder'</span><span class="p">})</span>
<span class="n">df2_g</span><span class="p">[</span><span class="s1">'Murder per capita'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df2_g</span><span class="p">[</span><span class="s1">'Murder'</span><span class="p">]</span> <span class="o">/</span> <span class="n">df2_g</span><span class="p">[</span><span class="s1">'Population'</span><span class="p">]</span>
<span class="n">df2_g</span><span class="p">[</span><span class="s1">'Crime per capita'</span><span class="p">]</span> <span class="o">=</span>  <span class="n">df2_g</span><span class="p">[</span><span class="s1">'Total crime'</span><span class="p">]</span> <span class="o">/</span> <span class="n">df2_g</span><span class="p">[</span><span class="s1">'Population'</span><span class="p">]</span>

<span class="c1">#</span>
<span class="c1"># Nashville-Davidson has '       ' as value</span>
<span class="c1"># (error in the data source file, the cell was not completely merged)</span>
<span class="c1"># Nevertheless, that row can be dropped without affecting the result</span>
<span class="c1">#</span> 
<span class="c1"># get just the total crime and population</span>

<span class="n">df2_g</span><span class="p">[</span><span class="s1">'Metro name'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df2_g</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">x</span><span class="o">.</span><span class="n">name</span><span class="p">[</span><span class="mi">0</span><span class="p">:</span><span class="o">-</span><span class="mi">7</span><span class="p">],</span> <span class="n">axis</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span>
<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2_g</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="s1">'Metro name'</span><span class="p">)</span>
<span class="n">df2_g</span> <span class="o">=</span> <span class="n">df2_g</span><span class="p">[[</span><span class="s1">'Murder per capita'</span><span class="p">,</span><span class="s1">'Crime per capita'</span><span class="p">]]</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Data Preparation: get x, y axes[¶](#Data-Preparation:-get-x,-y-axes)

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [4]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="n">dfm</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">merge</span><span class="p">(</span><span class="n">df1</span><span class="p">,</span> <span class="n">df2_g</span><span class="p">,</span> <span class="n">how</span><span class="o">=</span><span class="s1">'inner'</span><span class="p">,</span> <span class="n">left_index</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">right_index</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>

<span class="n">x_ADH</span> <span class="o">=</span> <span class="n">dfm</span><span class="p">[</span><span class="s1">'ADH per capita'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="n">y_Crime</span> <span class="o">=</span> <span class="n">dfm</span><span class="p">[</span><span class="s1">'Crime per capita'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>

<span class="n">dfm_mur</span> <span class="o">=</span> <span class="n">dfm</span><span class="o">.</span><span class="n">drop</span><span class="p">((</span><span class="n">dfm</span><span class="p">[</span><span class="n">dfm</span><span class="p">[</span><span class="s1">'Murder per capita'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">0</span><span class="p">])</span><span class="o">.</span><span class="n">index</span><span class="p">,</span><span class="n">axis</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">x_ADH_mur</span> <span class="o">=</span> <span class="n">dfm_mur</span><span class="p">[</span><span class="s1">'ADH per capita'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="n">y_Murder</span> <span class="o">=</span> <span class="n">dfm_mur</span><span class="p">[</span><span class="s1">'Murder per capita'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>

<span class="n">x_Mur_min</span> <span class="o">=</span> <span class="n">x_ADH_mur</span><span class="o">.</span><span class="n">min</span><span class="p">()</span>
<span class="n">x_Mur_max</span> <span class="o">=</span> <span class="n">x_ADH_mur</span><span class="o">.</span><span class="n">max</span><span class="p">()</span>

<span class="n">y_Mur_min</span> <span class="o">=</span> <span class="n">y_Murder</span><span class="o">.</span><span class="n">min</span><span class="p">()</span>
<span class="n">y_Mur_max</span> <span class="o">=</span> <span class="n">y_Murder</span><span class="o">.</span><span class="n">max</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [5]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="n">x_ann</span> <span class="o">=</span> <span class="n">dfm</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span><span class="s1">'Ann Arbor, MI'</span><span class="p">,</span> <span class="s1">'ADH per capita'</span><span class="p">]</span>
<span class="n">y_c_ann</span> <span class="o">=</span> <span class="n">dfm</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span><span class="s1">'Ann Arbor, MI'</span><span class="p">,</span> <span class="s1">'Crime per capita'</span><span class="p">]</span>
<span class="n">y_m_ann</span> <span class="o">=</span> <span class="n">dfm</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span><span class="s1">'Ann Arbor, MI'</span><span class="p">,</span> <span class="s1">'Murder per capita'</span><span class="p">]</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Plotting Results[¶](#Plotting-Results)

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [6]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="n">sns</span><span class="o">.</span><span class="n">set_style</span><span class="p">(</span><span class="s1">'white'</span><span class="p">)</span>

<span class="n">x1_str</span> <span class="o">=</span> <span class="s1">'All-religions adherents per capita'</span>
<span class="n">y1_str</span> <span class="o">=</span> <span class="s1">'All-crimes per capita'</span>

<span class="n">ax1</span> <span class="o">=</span> <span class="p">(</span><span class="n">sns</span><span class="o">.</span><span class="n">jointplot</span><span class="p">(</span><span class="n">x_ADH</span><span class="p">,</span> <span class="n">y_Crime</span><span class="p">,</span> <span class="n">kind</span> <span class="o">=</span> <span class="s1">'scatter'</span><span class="p">,</span> <span class="n">alpha</span> <span class="o">=</span> <span class="mf">0.75</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">1</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">'All data'</span><span class="p">)</span>
        <span class="o">.</span><span class="n">set_axis_labels</span><span class="p">(</span><span class="n">x1_str</span><span class="p">,</span> <span class="n">y1_str</span><span class="p">,</span> <span class="n">size</span> <span class="o">=</span> <span class="mi">15</span><span class="p">))</span>
<span class="c1"># plt.scatter(x_ann,y_c_ann, color = 'r',zorder = 3)</span>

<span class="n">ymin1</span><span class="p">,</span> <span class="n">ymax1</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">ylim</span><span class="p">()</span>
<span class="n">xmin1</span><span class="p">,</span> <span class="n">xmax1</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">xlim</span><span class="p">()</span>

<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">16</span><span class="p">)</span>

<span class="n">fig</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gcf</span><span class="p">()</span>
<span class="n">fig</span><span class="o">.</span><span class="n">set_size_inches</span><span class="p">(</span><span class="mi">8</span><span class="p">,</span><span class="mi">8</span><span class="p">)</span>

<span class="n">ax_total</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">get_axes</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">axes</span><span class="p">(</span><span class="n">ax_total</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gca</span><span class="p">()</span>
<span class="n">ax</span><span class="o">.</span><span class="n">xaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">yaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">left</span> <span class="o">=</span> <span class="mf">0.1</span><span class="p">)</span>
<span class="n">ax1</span><span class="o">.</span><span class="n">ax_joint</span><span class="o">.</span><span class="n">plot</span><span class="p">([</span><span class="n">x_ann</span><span class="p">],[</span><span class="n">y_c_ann</span><span class="p">],</span><span class="s1">'ro'</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">'Ann Arbor'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">(</span><span class="n">prop</span><span class="o">=</span><span class="p">{</span><span class="s1">'size'</span><span class="p">:</span><span class="mi">12</span><span class="p">},</span> <span class="n">frameon</span> <span class="o">=</span> <span class="kc">True</span><span class="p">,</span> <span class="n">facecolor</span> <span class="o">=</span><span class="s1">'w'</span><span class="p">,</span> <span class="n">edgecolor</span> <span class="o">=</span> <span class="s1">'k'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">savefig</span><span class="p">(</span><span class="s2">"2010_s_all_crime.png"</span><span class="p">)</span>

<span class="c1">#plot hex graph</span>
<span class="n">ax1</span> <span class="o">=</span> <span class="n">sns</span><span class="o">.</span><span class="n">jointplot</span><span class="p">(</span><span class="n">x_ADH</span><span class="p">,</span> <span class="n">y_Crime</span><span class="p">,</span> <span class="n">kind</span> <span class="o">=</span> <span class="s1">'hex'</span><span class="p">)</span><span class="o">.</span><span class="n">set_axis_labels</span><span class="p">(</span><span class="n">x1_str</span><span class="p">,</span> <span class="n">y1_str</span><span class="p">,</span> <span class="n">size</span> <span class="o">=</span> <span class="mi">15</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylim</span><span class="p">(</span><span class="n">ymin1</span><span class="p">,</span> <span class="n">ymax1</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlim</span><span class="p">(</span><span class="n">xmin1</span><span class="p">,</span> <span class="n">xmax1</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">16</span><span class="p">)</span>

<span class="n">fig</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gcf</span><span class="p">()</span>
<span class="n">fig</span><span class="o">.</span><span class="n">set_size_inches</span><span class="p">(</span><span class="mi">8</span><span class="p">,</span><span class="mi">8</span><span class="p">)</span>

<span class="n">ax_total</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">get_axes</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">axes</span><span class="p">(</span><span class="n">ax_total</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gca</span><span class="p">()</span>
<span class="n">ax</span><span class="o">.</span><span class="n">xaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">yaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">left</span> <span class="o">=</span> <span class="mf">0.1</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">savefig</span><span class="p">(</span><span class="s2">"2010_h_all_crime.png"</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_png output_subarea ">![](data:image/png;base64,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
RK5CYII=
)</div>

</div>

<div class="output_area">

<div class="output_png output_subarea ">![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAjUAAAI7CAYAAAAUH2roAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAIABJREFUeJzs3XuUXGWd7//3rvutq6/phNyTDpCQACFEJMIP1phzJIOM
KIPjMcssnaNH0PGcNS6ZIwxjgDDqqGuCnkGZOYhnCZwRGcb5OV5+/jiTnwtHJSIXIeRCTLpz6SR9
7677fT+/P6qrSSedpG9V1an6vNbC2FXV9TxPVfXen/3dT+3HMsYYRERERC5yjmp3QERERGQ2KNSI
iIhITVCoERERkZqgUCMiIiI1QaFGREREaoJCjYiIiNQEhRoRERGpCQo1IiIiUhMUakRERKQmuKrd
AZF69rMXj1SsrS2bllesLRGRalClRkRERGqCQo2IiIjUBIUaERERqQkKNSIiIlITFGpERESkJijU
iIiISE1QqBEREZGaoFAjIiIiNUGhRkRERGqCQo2IiIjUBIUaERERqQla+0nmtEqujQRaH0lE5GKm
So2IiIjUBIUaERERqQk6/SRymkqf7hIRkdmjUCNTop2+iIjMVTr9JCIiIjVBoUZERERqgkKNiIiI
1ASFGhEREakJCjUiIiJSE/TtJ5E6oaszi0itU6VGREREaoJCjYiIiNQEhRoRERGpCQo1IiIiUhMU
akRERKQmKNSIiIhITVCoERERkZqgUCMiIiI1QaFGREREaoKuKFwDKn2lWBERkblIlRoRERGpCQo1
IiIiUhMUakRERKQmKNSIiIhITVCoERERkZqgUCMiIiI1QaFGREREaoJCjYiIiNQEhRoRERGpCQo1
IiIiUhMUakRERKQmKNSIiIhITVCoERERkZqgVbpFpCwqvXr8lk3LK9qeiMw9qtSIiIhITVCoERER
kZqgUCMiIiI1QaFGREREaoImCpdBpSdIioiIiCo1IiIiUiMUakRERKQmKNSIiIhITdCcGhGRKdKF
BUXmJlVqREREpCaoUiMiNUHfOhQRVWpERESkJijUiIiISE1QqBEREZGaUBdzanSuXUREpPapUiMi
IiI1QaFGREREakJdnH4SEbmY6WJ/IpOjSo2IiIjUBIUaERERqQkKNSIiIlITNKdGRETGqeQcnkrP
39H8pNqmSo2IiIjUhJqv1OTzeQb7e6rdDRERmUB3d2V3Q5XeH3R3u1iwYAEuV83vbucEyxhjqt2J
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
)</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [ ]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span> 
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [7]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="n">y1_str</span> <span class="o">=</span> <span class="s1">'Murder cases per capita'</span>

<span class="n">ax1</span> <span class="o">=</span> <span class="p">(</span><span class="n">sns</span><span class="o">.</span><span class="n">jointplot</span><span class="p">(</span><span class="n">x_ADH_mur</span><span class="p">,</span> <span class="n">y_Murder</span><span class="p">,</span> <span class="n">kind</span> <span class="o">=</span> <span class="s1">'scatter'</span><span class="p">,</span> <span class="n">alpha</span> <span class="o">=</span> <span class="mf">0.75</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">'All data'</span><span class="p">)</span>
        <span class="o">.</span><span class="n">set_axis_labels</span><span class="p">(</span><span class="n">x1_str</span><span class="p">,</span> <span class="n">y1_str</span><span class="p">,</span> <span class="n">size</span> <span class="o">=</span> <span class="mi">15</span><span class="p">))</span>

<span class="n">ax1</span><span class="o">.</span><span class="n">ax_joint</span><span class="o">.</span><span class="n">plot</span><span class="p">([</span><span class="n">x_ann</span><span class="p">],[</span><span class="n">y_m_ann</span><span class="p">],</span><span class="s1">'ro'</span><span class="p">,</span><span class="n">zorder</span> <span class="o">=</span> <span class="mi">3</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">'Ann Arbor'</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">ylim</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="n">y_Mur_max</span><span class="p">)</span>
<span class="n">ymin1</span><span class="p">,</span> <span class="n">ymax1</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">ylim</span><span class="p">()</span>
<span class="n">xmin1</span><span class="p">,</span> <span class="n">xmax1</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">xlim</span><span class="p">()</span>

<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">16</span><span class="p">)</span>

<span class="n">fig</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gcf</span><span class="p">()</span>
<span class="n">fig</span><span class="o">.</span><span class="n">set_size_inches</span><span class="p">(</span><span class="mi">9</span><span class="p">,</span><span class="mi">8</span><span class="p">)</span>

<span class="n">ax_total</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">get_axes</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">axes</span><span class="p">(</span><span class="n">ax_total</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gca</span><span class="p">()</span>
<span class="n">ax</span><span class="o">.</span><span class="n">xaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">yaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">left</span> <span class="o">=</span> <span class="mf">0.12</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">(</span><span class="n">prop</span><span class="o">=</span><span class="p">{</span><span class="s1">'size'</span><span class="p">:</span><span class="mi">12</span><span class="p">},</span> <span class="n">frameon</span> <span class="o">=</span> <span class="kc">True</span><span class="p">,</span> <span class="n">facecolor</span> <span class="o">=</span><span class="s1">'w'</span><span class="p">,</span> <span class="n">edgecolor</span> <span class="o">=</span> <span class="s1">'k'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">savefig</span><span class="p">(</span><span class="s2">"2010_s_murder.png"</span><span class="p">)</span>

<span class="c1">#plot hex graph</span>
<span class="n">ax1</span> <span class="o">=</span> <span class="n">sns</span><span class="o">.</span><span class="n">jointplot</span><span class="p">(</span><span class="n">x_ADH_mur</span><span class="p">,</span> <span class="n">y_Murder</span><span class="p">,</span> <span class="n">kind</span> <span class="o">=</span> <span class="s1">'hex'</span><span class="p">)</span><span class="o">.</span><span class="n">set_axis_labels</span><span class="p">(</span><span class="n">x1_str</span><span class="p">,</span> <span class="n">y1_str</span><span class="p">,</span> <span class="n">size</span> <span class="o">=</span> <span class="mi">15</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">ylim</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="n">y_Mur_max</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlim</span><span class="p">(</span><span class="n">xmin1</span><span class="p">,</span> <span class="n">xmax1</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">16</span><span class="p">)</span>

<span class="n">fig</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gcf</span><span class="p">()</span>
<span class="n">fig</span><span class="o">.</span><span class="n">set_size_inches</span><span class="p">(</span><span class="mi">9</span><span class="p">,</span><span class="mi">8</span><span class="p">)</span>

<span class="n">ax_total</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">get_axes</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">axes</span><span class="p">(</span><span class="n">ax_total</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span>
<span class="n">plt</span><span class="o">.</span><span class="n">yticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">size</span> <span class="o">=</span> <span class="mi">13</span><span class="p">)</span>
<span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">gca</span><span class="p">()</span>
<span class="n">ax</span><span class="o">.</span><span class="n">xaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">yaxis</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">color</span><span class="o">=</span><span class="s1">'g'</span><span class="p">,</span> <span class="n">linestyle</span><span class="o">=</span><span class="s1">'--'</span><span class="p">,</span> <span class="n">linewidth</span><span class="o">=</span><span class="mf">0.4</span><span class="p">,</span> <span class="n">zorder</span> <span class="o">=</span> <span class="mi">0</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">left</span> <span class="o">=</span> <span class="mf">0.12</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">savefig</span><span class="p">(</span><span class="s2">"2010_h_murder.png"</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_png output_subarea ">![](data:image/png;base64,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
M5g5cyY+++wzn3Pu3r0bhYWFWLFiBfbt24d77rkHM2fOxKVLlxT+rhCpj9PlxvvbT2Dpqr3e/97f
fgJOlzvYXSMiElrAkr7du3dDo9Fg6tSpiIiIwJQpU5CSkoIvv/zSZ79NmzYhNzcXgwcPRnR0NB57
7DF89dVXMJlMrR6jtrYWW7duxZw5cxAVFYVBgwYhLy8PGzZsAADYbDZMnjwZV199NYYMGeJzzvLy
ctx3333o378/NBoNJk2aBK1WixMnTvgl9mE9hwlTM4mxqlN7Yt24owR7DpfDYnUiQqeFxerEnsPl
2LijROFe+gevqzqJFCsgXrwkT8CKM5eWlsJoNPpsy8jIwMmTJ322nTx50icpS0pKQkJCAkpLS1s9
Rnp6OnQ6HXr16uXTtmXLFgCATqfD5s2b0bVrV0ybNs3nGBMnTvT5ev/+/aitrW1yruYU/ViE85rz
3q9TDClIS0yrP87Z/U32Zzvb1dyeEJWEohITNJKEc/Zin7YtR0/gmms0yEzuHbL9Zzvb2e6/diad
oSdgSZ/ZbIZer/fZFh0djbq6Op9tFosF0dHRPtv0ej0sFkurxzCbzU1e1/D4Go0GXbt2bbOfJ06c
wJw5czBnzhx06dJFdnytOVd9DgDQI66HX44Xyk5XnQYA7w++mol0XT2xphhSWt2vxmxHVa0NETpt
k7baOjtqzHYgWZEu+o1Itc1Eew/XOeqE+N0EiHVtSb6AJX16vb5JgldXVweDweCzraVE0GAwtHoM
vV4Pq9Xa5vFb8/XXX+ORRx7B9OnT8cADD8h6zcBuA5HaM7XZNs9fOZ6/iBr/1dPWX0Hh2L7/7H6Y
zCakJaaFZP/82e75ZdrSfsHunz/bPe/hhh+Yzb3e7nAiPuYSLFYnekT082nTR2kxoEemIv3zZ3tL
P6+BOn8g21uLNRT658/2xiNTodY/f7eH+u8nCo6A3dPXu3dvlJaW+mwrLS1FZmamzzaj0eizX0VF
BSorK2E0Gls9RlpaGux2O86ePdvq8Vvyj3/8A3PmzEF+fj5mz57d3vCICECETouBxhS43L6LNlxu
NwYaU5odASQiosAIWNI3cuRI2Gw2rF69Gna7HevXr4fJZMKoUaN89svLy8OWLVuwb98+WK1WFBYW
Ijs7G0lJSa0eIzY2Frm5uVi+fDksFgsOHjyIzZs3Y9y4cW32bdeuXfjjH/+IV199FXl5eUp9C4iE
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
)</div>

</div>

<div class="output_area">

<div class="output_png output_subarea ">![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAn0AAAI7CAYAAACZci28AAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAIABJREFUeJzs3XmYFPWdB/53VfXdPUfPdM89MDDIqRyCrBjikYnH7yfE
iKxGNsQrEQwEVyObqI8Q8Ygnu7rZNVnJ4Wry24ghbsRslsizUVlADiWuwIA4wzEzzNFz9EzfR31/
fww90g7M9BxdPTP1fj2PzyPV3fX9VnUd7/nWp6olIYQAEREREY1pcqY7QERERETpx9BHREREpAMM
fUREREQ6wNBHREREpAMMfUREREQ6wNBHREREpAMMfUREREQ6wNBHREREpAMMfUREREQ6YMh0B4go
/f6067jmbV63oELzNomI6Pw40kdERESkAwx9RERERDrA0EdERESkAwx9RERERDrA0EdERESkAwx9
RERERDrA0EdERESkAwx9RERERDrA0EdERESkAwx9RERERDrAn2EjorTQ+qff+LNvRER940gfERER
kQ4w9BERERHpAEMfERERkQ6wpo90T+vaMyIiokzgSB8RERGRDjD0EREREekAQx8RERGRDjD0ERER
EekAQx8RERGRDjD0EREREekAQx8RERGRDjD0EREREekAQx8RERGRDjD0EREREekAQx8RERGRDjD0
EREREemAIdMdIPqiP+06nukuEBERjTkc6SMiIiLSAYY+IiIiIh1g6CMiIiLSAYY+IiIiIh1g6CMi
IiLSAd69S0RjQibu+r5uQYXmbRIRDRZH+oiIiIh0gCN91Cc+M4+IiGhs4EgfERERkQ4w9BERERHp
AEMfERERkQ4w9BERERHpAEMfERERkQ4w9BERERHpAEMfERERkQ4w9BERERHpAEMfERERkQ4w9BER
ERHpAEMfERERkQ4w9BERERHpAEMfERERkQ4w9BERERHpAEMfERERkQ4YMt0BIqLR6k+7jmva3nUL
KjRtj4jGFoa+UUbrkwwRERGNDby8S0RERKQDHOkjIqLz4iVsorGDI31EREREOsDQR0RERKQDvLxL
RDRK8EYuIhoKjvQRERER6QBDHxEREZEO8PLuEPBSCxEREY0WHOkjIiIi0gGGPiIiIiIdYOgjIiIi
0gGGPiIiIiIdYOgjIiIi0gGGPiIiIiId4CNbiIhI1zLx+K3rFlRo3iYRQx8REY0YfP5pejDYEsDQ
N2ixWAytLY2Z7gYREY1CdXXann4zcb6qqzOgqKgIBgOjxkghCSFEpjsxGtXV1aGqqirT3SAiIhqx
tm/fjrKyskx3g85g6BukWCyGxkaO9BEREZ0PR/pGFoY+IiIiIh3gI1uIiIiIdIChj4iIiEgHGPqI
iIiIdIChj4iIiEgHGPqIiIiIdIChj4iIiEgHGPqIiIiIdIChj4iIiEgHGPqIiIiIdIChj4iIiEgH
GPqIiIiIdIChb5BisRjq6uoQi8Uy3RUiIqIxg+fX9DFkugOjVWNjI6qqqrB9+3aUlZVlujvUh+oT
7YjGVM3bdWaZUeKyQ5YlzdsmIhqtEufXdc++gnx3EQDgugUVme3UGMGRPiIiIiIdYOgjIiIi0gGG
PiIiIiIdYOgjIiIi0gGGPiIiIiIdYOgjIiIi0gGGPiIiIiIdYOjTmKoKNLYGUH2iHa3eIIQQme7S
mFdZko1il03T5+UZDTLyss2Q+Ig+IiIaIfhwZo0IIeD1RdDg8UMVAkIAp1sD8HSEUFrggMNq1LxP
qlAhS2M/9xuNCvKyLHBmmdHYGkBbZzhtbckSUOC0Ij/HysBHY44QApKON+yf/OQn+Mtf/gKDwYCH
HnoIM2fO7PWeeDyO++67D0uXLsXll18OAHj66afx4YcfIhaL4ZZbbsHNN9+c0X4eOHAATzzxBBRF
wcKFC7F69WoAwD333IP29nYYjUaYzWZs2rQJDQ0NeOihhxCPxyGEwIYNGzBx4sS09p/Sh6FPA4FQ
FHUtfkSicZw9sCcEEImpOH66E3aLEaVuO0xGRbN+SZB0cxDvHuWTUJxvhzvXiroWH/zB4f2Jn9ws
M0rybZAkib/CQWOO3q9KHDx4EHv27MHmzZvR0NCANWvW4He/+13Se06ePIl/+Id/QFNTE5YuXQoA
2L17N06ePInf/va3iEQiuP7663HttdciJycn7f08ffo0vve97/Xq5/r16/HP//zPKC8vx913341D
hw5h+vTpOHHiBN5+++2kc8ILL7yAb37zm/jqV7+K999/Hxs3bsRPfvKTtPSd0o+hL42isTgaPAF0
BSLo63gpBOALRnH0VAfysi0ozLNB0SA06CHsfZEsSzDJCiqKshEIRVHf4kdkiD/RZrMYUOa2w2hQ
GPZozBrM8WLLli1455134Pf70d7ejlWrVuHaa6/Fnj178I//+I9QFAXl5eXYsGEDwuEwHn74YXR1
daG5uRnLli3DsmXLsHz5cuTl5cHr9WLdunV46KGHYDAYoKoqnn/+eRQXF+Opp57C/v37AQCLFi3C
bbfdhh/+8IcwmUyor69Hc3MznnrqKcyYMQNXXXUVJk6ciMrKSjz00EM9fV2xYgUCgUDPvysrK/Gj
H/2o59/79+/HwoULIUkSSktLEY/H0dbWhry8vJ73BAIBPPHEE3j55Zd7ps2ZMwfTpk3r+Xc8HofB
YMB7772H6upq3H333T2v1dXV4d5774Xb7UZTUxMuv/xy3HfffUnrdCD9LCkp6dVPn8+HSCSCcePG
AQAWLlyInTt3oqCgAJ2dnVi5ciU6Oztx991346qrrsIPfvADZGVl9fTdbDanvgHQiMPQlwaqKtDc
HoTHG+wz7H2REEBbZwjtXWEU59ngzDbrMphpQZYl2K1GXFCei7bOEJrag1DVgY1kGA0ySlx2OKxG
hj2i8wgGg/jlL3+JtrY2/O3f/i2+8pWv4JFHHsFvfvMb5Ofn45/+6Z/w+9//HjNmzMD111+Pa665
Bk1NTVi+fDmWLVsGoDvIXX311fj1r3+NmTNnYu3atdi3bx+6urpQXV2Nuro6vP7664jFYli2bBku
vfRSAEBJSQk2bNiA119/Hb/97W+xYcMGnD59Glu2bIHT6Uzq589+9rM+l8Pn8yE3N7fn33a7HV1d
XUmhb+rUqb0+ZzabYTabEY1G8cMf/hC33HIL7HY7Lr/88p7Lv2err6/Hz3/+c2RlZWHZsmU4ePAg
ZsyYMWz99Pl8cDgcSa+fOnUK0WgUd955J771rW/B6/Xi1ltvxcyZM5Gfnw8AqKmpwdNPP41/+Zd/
6bN9GtkY+oaREAJefwQNLZ/X7Q18Ht3zOd3qR4s3iDK3A/YM1PvpgSRJkCQgL7u73u90qx/tXZF+
PydLgNtphetM3R6D+diVuKTJ73jwLrnkEsiyDJfLhezsbDQ3N6O5uRl///d/DwAIhUK47LLLcMUV
V+CVV17Btm3b4HA4EIt9Xn4xYcIEAMDSpUvx8ssv49vf/jaysrJw33334bPPPsO8efMgSRKMRiNm
zZqFzz77DAB6RtiKiorw4YcfAgCcTmevwAf0P4LmcDjg9/t7/u33+3tGwPrj9XqxZs0azJ8/HytW
rOjzvVOnTu0JbTNnzkRtbW1S6BtqP8/1enZ2NlwuF77xjW/AYDAgPz8f06ZNQ21tLfLz87F79248
+uijeOaZZ3rV8+mlRGisYOgbJoFQDPUtPoS/ULc3WKoAIlEVtRmq99OTnno/lx1upw31zT74Q+eu
98t1mFDsskNm3Z5u8IQ2NAcPHgQAeDwe+Hw+FBUVoaioCP/6r/+KrKwsbN++HTabDb/4xS8we/Zs
LFu2DLt378a7777bM4/Ed7B9+3bMnTsXq1evxtatW7Fp0yZcc8012LJlC26//XZEo1F89NFHuPHG
G5M+dzZZPvfNa/2NoF188cV49tlncdddd6GxsRGqqiaN8p1PKBTC7bffjjvuuANf+9rX+n3/Z599
hmAwCJPJhI8//hg33XTTsPbT4XDAaDTi5MmTKC8vx44dO7B69Wrs3LkTr732Gl5++WX4/X58+umn
mDhxInbv3o0nnngCmzZtQmlp6TnbZPAbPRj6higWFzjZ2IXOfur2Buvser/8bAsKNKr30yNFlqHI
QEVxNvxn6v2iZ+r9bGYDSgvsMLFuT1d4Ihs6j8eD2267DV1dXVi/fj0URcHDDz+Mu+++G0II2O12
PPPMM5AkCY8//jj++Mc/IisrC4qiIBJJHnm/8MIL8YMf/AAvvfQSVFXFgw8+iBkzZmDPnj245ZZb
EI1Gcd111yWNjA2XCy+8EPPmzcMtt9wCVVWxbt06AMCuXbuwf//+njtgv+g//uM/cOrUKWzevBmb
N28GADz55JOora3tVdMHAEajEffeey88Hg+uu+66c14yHmo/H330UTzwwAOIx+NYuHAhZs2aBQDY
sWMHbr75ZsiyjPvvvx95eXl48skney5NA92jrhs2bOhpj/vI6CIJvd+SNUh1dXWoqqrCS7/6HdwF
xZq0mbiUWF5gR7adxbTpJM5cnu/oCsFgUOCwGnkpl2iAtmzZgpqaGjzwwAOZ7sqI09rais2bN2Pl
ypU90+rq6nD//ffj9ddfz2DPMi9xfl337CvIdxcBAK5bUJHZTo0RHOkbIi0jc6Lez2rm15ZuiXo/
Z7al5996wTo2ovQTQuDOO+/MdDdIZ5geRiGejLWj13Wt1+Wm4bVkyZJMd2HEcrlcvaaVlZXpfpSP
0mvs/xwDERERETH0EREREekBQx8RERGRDjD0EREREekAQx8RJeFNHEREYxNDHxEREZEOMPQRURI+
r52IaGxi6CMiIiLSAYY+IiIiIh1g6CMiIiLSAYY+IiIiIh3gb+8SERHRiHTdgopMd2FM4UgfESXh
c/qIiMYmhj4iIiIiHWDoI6IkfE4fEdHYxNA3CgkheGKmtOL2RUQ09jD0DZEsAVpVQEkSoMgSQpE4
665o2KlCIK6qaGwLwBeMQlUZ/IiIxhLevTtEE0tzoFjM6PCFkc7BEUkC3DlWuJ1WyDIDHw2f7pFj
oK0rjKa2AFRVwNMRgsNqRKnbDoMic5sjIhoDGPqGSJEllBU44MqxoK7Fj1AkNqzhT5KALJsJJS47
jAYOzKaTEEJ3I6iqKhAIR1Hf4kckqia95gtGceRkB/KyzSjKt0GSJMg6Wz963CaIBiJRCsL9ZHRg
6BsmFrMBlaXZ6ApEUd/iQ1wVQwp/kgSYjQpK3Q7YLPya0k1vJ3dVFYjFVdS3+OELRvt8b1tnGB2+
CIrybHBmmSFJ+jnACwhA6Gd5iQaD+8fowTQxjCRJQrbdBIfNidaOIJrbgxhoWZQkAbIkocRlR47D
xJ1JI3pZz+qZm4AaWwNo6wyn/jlVoMHjh8cbRKnbDpvZqItLvrLE0XWivujl2DlWMPSlgSxJcDtt
cGZbcLrVD68vktKonyQBrhwrCli3R8MsUbfX3hVG45m6vcGIRFXUNnSx3o+IaBRi6EsjgyKjvCAL
rpwY6lt8CEfi5xz5+7xuzwajQdGsf6zF0IdE3V5Dix/hL9TtDRbr/fSHx4vPcV3QaMXQpwGr2YDK
0pxe9X6yBBiNCsrcdtgsRs37JSB4+WoMG0jd3mC1dYbh9UVQeFa9H8CT4VgkSRKf33gGt28arRj6
NPLFer+2rjAKnbaM1u0x8I1d4UgMrd4wWjtDaW8rfqber9UbwvjiLJh4l/mYxbBDNLox9GksUe/n
dtoy3RUaw46e8mreZjgaR6c/AneuVfO2iYiof/yTnIiIiEgHGPqIiIiIdIChj4iIiEgHGPqIiIiI
dIChj4iIiEgHGPqIiIiIdIChj4iIiEgHGPqIiIiIdEDT0Hfo0CEsXboUs2fPxg033IADBw6c831b
t25FVVUVZs+ejRUrVsDj8aQ0D6/Xi1WrVmHu3Lm48sorsXnz5l7zVlUVq1evxmuvvZY0/fXXX8c1
11yDiy++GDfddBP27ds3TEtNRERElHmahb5wOIyVK1diyZIl2Lt3L5YvX4577rkHfr8/6X3V1dVY
v349Nm7ciN27d8PlcuHBBx9MaR6PPPIIbDYbdu7ciRdffBHPPfdcUiisr6/HypUr8ec//zmpzd27
d2Pjxo144YUXsG/fPnzzm9/EypUr0d7enua1QkRERKQNzULf7t27Icsyli1bBqPRiKVLl8LlcuHd
d99Net9bb72FqqoqzJo1CxaLBQ888ADef/99eDyePufh9/vxzjvvYM2aNTCbzZg5cyYWLVqEN998
EwAQiUSwZMkSTJ48GXPmzElqs7GxEXfddRemTZsGWZZx4403QlEUHDt2TKvVQ0RERJRWmv32bm1t
LSorK5OmTZgwATU1NUnTampqkkKZ0+lETk4Oamtr+5xHRUUFDAYDysvLk17btm0bAMBgMGDr1q1w
u91Yvnx50jy+/vWvJ/17//798Pv9vdoaC1ShQpYyU8ophMjID7Znql0gs+s7M0TmWs7Q96zHfUqP
9Hj84vY19mh2pAoEArBak3+I3WKxIBQKJU0LBoOwWCxJ06xWK4LBYJ/zCAQCvT539vxlWYbb7e63
n8eOHcOaNWuwZs0a5OXlpbx8RERERCOZZqHParX2CnihUAg2my1p2vmCoM1m63MeVqsV4XC43/n3
ZceOHbj11lvxd3/3d7j77rtT/txokslRp0z9xZjJv1T1NcoHAJlb15n6nvW4T+mRHo9f3L7GHs2O
VhMnTkRtbW3StNraWkyaNClpWmVlZdL72tra4PV6UVlZ2ec8xo8fj2g0ioaGhj7nfz6/+93vsGbN
Gqxfvx7f/e53B7p4RERERCOaZqFvwYIFiEQiePXVVxGNRvHGG2/A4/Fg4cKFSe9btGgRtm3bhn37
9iEcDmPjxo24/PLL4XQ6+5yHw+FAVVUVnn/+eQSDQXz88cfYunUrFi9e3G/fdu3ahUcffRT/9m//
hkWLFqVrFRBpptRth9WsWcluD58/gub2AFSRudo+IiI6N81Cn8lkwssvv4y3334b8+fPx2uvvYaX
XnoJNpsN69atw7p16wAA06ZNw2OPPYaHH34YCxYsQHNzM3784x/3Ow8AeOyxxxCLxXDFFVdgzZo1
WLt2LWbNmtVv315++WVEo1F85zvfwZw5c3r+e++999K3QojSyJllxsSSbIwrdMCgaHf50ReKobk9
iCMn2tHpj0Aw/I0p/D6JRjdJcC8elLq6OlRVVWH79u0oKyvLdHcGRQgBAaHDujP9EEJACKClI4iW
jiC03NtlCTCbFJS5HbBkYNSRhlfiVME6Lx470y1xfl337Cv4u69dmunujCncYnVMkiQetMY4SZIg
yxLcuVZMHe9EjsOkWduqAILhOI7Ve3GquQuxuKpZ2zT8JEli4DuDx04arbjVEumALEswKDLK3A5c
UJajab2fEEBHVwRHTrSjhfV+REQZw9BHBP3UKsmyBLNJyUi9nyow6ur9ui+Pj/x+EmUK94/RhYU2
RGfo5enz3ZfpgGy7CVk2k6b1fqoA1LjAyaYuWEwGlLntI77eTw/bBNFQ6OXYORZwpI8I+qxXymS9
nxBAMBw7U+/nG7H1fnrbJogGSo/HztGMoY9I586u95tUlgOLSYaANiFMCMDbFT5T7xdkvR8RURox
9BERgO7wZzEpMCgyJA1/Tk2g+7JvVzCiWZtERHrE0EdEPSRJQjSmIhO/oavIcncCJCKitGDoI6Ik
rM8hIhqbGPqIiIiIdIChj4iSCF5jJSIakxj6iCgZMx8R0ZjE0EdERESkAwx9RERERDrA0EdERESk
Awx9RERERDrA0EdESficPiKisYmhj4iIiEgHGPqIKAmf00dENDYx9BFRMmY+IqIxiaGPiIiISAcY
+oiIiIh0gKGPiIiISAcY+oj6IAQL3LSkx7XNbYyItMLQR3QesbiKTn8EwXAMsbia6e5oQlUFsuwm
zduNqypaO4M47fEjEo1r3n4mqKpAOBpHJKZCVRn8iCj9DJnuANFIE4uriMVU7D3chLpmH2QJuGCc
ExdVuiDLEhR57D28WFUFojEVdS0+BEIxzdoVQiCuChyubcPh422IqwIXlOfixisqYbcYYDQqmvVF
K6oqIITA6dYA2rvCAIBchwnFLjtkSYI8BrcvIhoZGPqIzlBVFaoADtW04vCJ9p7RF1UAR060o7ah
E3MmuzCuKBuKLI2JX644VwDRQiLsNXh8+LC6GcHw56N7n57qwHO/+RCXzijE1fPHw6BIUJTRf1FC
CAEhgLbOEJrag0mjex2+CDr9EbidVrhyrJAk/jIKjR5CiLRtr3/adbzf91y3oCItbY9FDH2ke4kA
UtfchY+OtCAUOfflxUg0jg8ONqH6RDvmTy9CbpYZhlEaRhIBpNUbRHN7EFpeXYzFVfgCEXxwsBFt
necOmqoqsPP/GvHRUQ+uu3QcZk92Q5FlzUfBEvV2Qz2hqaqAPxRFQ4sfkdi5SwVUATS1BdHWGUaJ
yw6H1chRPxrxWJM6ujD0ka7F4iq6/N0BJNWRLq8vgj/vOYkStx2XTCuCySiPqvCXCCD1LX5EzxNA
0iGuCsRicew73IyTTV0pfSYYjuH379Zgx19P48YrK1HissOk4SVfSZKgChUSBhe+VFUgGldR3+KD
P5jaZfNoTMWJxi7YLAaUuu0wGRSGPxqxOCI9ujD0kS7F4yqiMRX7qptwqsk3qHk0tPjxluczTD6r
3m8kn5x7AkizD/4M1O1VH2/Dodruur2BaukI4t/e/ASTx+Xi64l6P4M24U+WBh7oVVVAQOC0Z/CX
zQOhGD495UVulhkl+TZIrPcjoiFi6NOx4bp0NZqoqoAqBA7VtuLw8fYh3zWpCqD6TL3f7ClujCvM
GnH1fpmu2zvt8WN/dTOC4aEHzaMnO/Dcrz/EggsL8dVLRl69X191e4PV0RVGpy+c8Xq/dNZtjTZ6
PHbS2MDQp3N6OWglAkh9S/eNA+er2xuscDSODz5pRPXxthFT79dTt9cZQnNbEKqGtTexuApfMIoP
PmlEW2doWOetqgL/+3EjPjziwf+zYDxmXeCCosiQM7wtq6pAIBRFvcePSHR4L5uPlHo/Br/PcT3Q
aMTQp2N6OWipQsDbFR5Q3d5gJer9St12/M2MIpiMSsZGZXzBzNXt7a9uxonG1Or2BisYjmHLXz7D
jr824KarJqGswJGxdZ143E2qdXuDdXa9X1mBAyaDrNky6+V4kQquCxqtGPpozPvTzlp4/VFN26xv
8eOvn3owd2oBFEX7E8ShYbh0PRgfH2vB0RMdmo4qNrcH8Zv/PoL7bp2j6U0eCcdPd8KX5rD3RYFQ
DPUtPowvyoLCAEJEKRo5xTBEaRKLZ+aRApl8kEGmfuFBCGga+EYCnS0uEY1iDH1EREREOsDQR0RE
RKQDDH1EREREOsDQR0RERKQDDH1EREREOsDQR0RERKQDDH1EREREOsDQR0RERKQDDH1EREREOsDQ
R0RERKQDDH1EREREOsDQR0RERKQDDH1EREREOsDQR0RERKQDDH005hXl22AxKZq3K0mAPxjVvF0A
yLabIEvat+vOtaLEZde8XYtZgZyJBQZgNRtgMmh/KFVVAV8gCiGE5m0T0ehkyHQHiNJt7rQiXDxV
4FBtKw4fb4eqpvckqcgSilx2mE0GnGz2weoNodRth8Wk3e5WXuCAEAKnWwNo7wqnvT2jQUap247p
FU6oQuDIiXa8taMWXYH0hl5ZlrDgwkJ89ZLxGQt9Rfk2FObZ0NYZQlN7MO3bV0IwHMepZh9MBgVl
BXbYLEZN2iWi0YuhT8cSIwSSlJmTpVYUWQIgYcaEfEwud2Lv4SbUNfuGvR0JQH6OBQV5dkhS93oV
AgiEYjhW54XTYUZhvg0GJf2jQvKZZS5x2eHOtaKuxYdAKDb87UhAgdOG/BxLzzIrAKZPyMfkcU68
f6Ae737UgFhcHfa2J4/LxdevqITdYoDRoP1IboIkSZAkIC/bAmeWWbOgDQBCAOFoHDUNnciyGVHi
sqdtXejleJEKIQTXA41Kml6TOHToEJYuXYrZs2fjhhtuwIEDB875vq1bt6KqqgqzZ8/GihUr4PF4
UpqH1+vFqlWrMHfuXFx55ZXYvHlzr3mrqorVq1fjtddeS7nNsUpvBy1FkWExG7DgomJce+l4OLPM
wzbvLJsRk8c7UZBvhyxLvdatEEB7VxhHTrTD0xHU7JKcLEswmxRMKM5GRXEWjMN4GdKZZcbU8U7k
51h6LbMsSzAZFVw+pww/WD4XF1XmD1u77lwr7v76hVh2zRTkOswZDXxnk2UJiiKjxGXH5HG5sFu0
+5taCKDTH8WRkx1obA2kbbRRb8eM8+n+g46X1Wn00Sz0hcNhrFy5EkuWLMHevXuxfPly3HPPPfD7
/Unvq66uxvr167Fx40bs3r0bLpcLDz74YErzeOSRR2Cz2bBz5068+OKLeO6555JCYX19PVauXIk/
//nPKbc51unxIG5QZDizzPjq/HFYcFHxkOr9zEYFE0tzUF6YDaNBgdzH+hQAVAE0tgVw5GQHugKR
Qbc7ULIswWE1YnJ5LoryrEOq97NZDJhcnosSlx2KIvd5WdVokGG3GnHTVZPwvb+dNaR6P6vZgK9f
MRGr/3YmxhVmwWQcGWHvi2RZgtmooOJM0Nay3k8IwOMNovpEOzq6wsMaTPR4rOgL1weNRpodjXbv
3g1ZlrFs2TIYjUYsXboULpcL7777btL73nrrLVRVVWHWrFmwWCx44IEH8P7778Pj8fQ5D7/fj3fe
eQdr1qyB2WzGzJkzsWjRIrz55psAgEgkgiVLlmDy5MmYM2dOym3S2CRJEgyKjHGFWVj85YmYPsHZ
Z2D7IkWWUFrgQGVZLmwW44DqyYQAojEVJxq7UFPvRSgSH8wiDJgkSZBlCfk5Vkwd7xzwSKfRIGN8
URYmFGfDbBrYjRMmo4KifBtW3Hghbr16MrJsqdefddftFeEfvjkXc6cUdIdrDev3BhucEkH7gvJc
FOfbBrR9DYUQQFwVqG/x4dNTXgRCmbmZiIhGHs2uP9TW1qKysjJp2oQJE1BTU5M0raamJimUOZ1O
5OTkoLb59Vt2AAAgAElEQVS2ts95VFRUwGAwoLy8POm1bdu2AQAMBgO2bt0Kt9uN5cuXp9ymy+Ua
0HIOZ63HQGpoVKFCgpTy+1NtP5V5DVetz0DnMxztyrIEGdKZGrQ87Euh3i8/x4LCPDsgYUgnciEA
fyiKY3UdyHWYUJzfPXLW92eGvn0l6v2KXTa4ci2ob/H3We8nS4DbaYUrxzqkZZYkCUaDgukT8zFl
vBPvfVSP9w70Xe93QXkubrxiImwW47CM7A1k/Q3HKNnn9X7mM/V+frR3pTbCKyB69unBUAUQjsZQ
09AJh9WIUnf/9X48fmnT5tntDkfbg5nXcH3XQggICMgSHwYyGmgW+gKBAKxWa9I0i8WCUCiUNC0Y
DMJisSRNs1qtCAaDfc4jEAj0+tzZ85dlGW63+5x966vNVKWjyDlRN9LXvBMHSwm968iGQ18HhuFe
5t51cP0flIarbaNBgdEALLioGJ3+MD442ISOLxTjO2xGlLodUOS+L2kOjNRT7+f1RVCQ1x2uzrUu
hpsiy1BMMiYUZ8MXjKDBE0A0lhzAcrO6w6gkYdiWWZElKLKCKy4uxaUXFeEP79Xik5rWpPe4ci24
8YpKlLodw3oZ9+z1quX2JcvdJ8Rilx1upw31zT74zxm0BZAIP0MIfJ/r3r66AhEcORnt/oPFaev1
XaZj+0qsu/6OE0KItB6/ztd2OoLm2fNRhdpvEBrudvv7Hs/e5oe1bZG5G32uW1ChaXujnWahz2q1
9gp4oVAINpstadr5gqDNZutzHlarFeFw+Jyv9aevNlOVrg29v/mm868rrU6II6n97no/C66ePw51
TT58dLQZQgClBQ5YTIY0XlaUoAqgqS2IVm8YpW47smymz19N47qWZQlZNhMml5vg8QbR0h6ExWxA
2ZmRoXQtc3fQVrD0K5Nw5cWl2PKXz9DWGcK1l47HxVPcwxyue8vE9qXIMhQZqCjOhj8URX2L/wtB
O33blxBAqzeE9s4wSlx25DhMwx4Aztlyho4jmTx2pjL/dCz3SF3XNHJoNh47ceJE1NbWJk2rra3F
pEmTkqZVVlYmva+trQ1erxeVlZV9zmP8+PGIRqNoaGjoc/7n0lebpD899X5FWVg4qwSTygdetzdY
iXq/U80+xNPwmJPzSdT7uXKsmFaRd6ZuL50h93Mmo4Jilx0rbrwQD912CeZN1b5uT2uJer+SfG0f
ZJ2o96tr8fUa1SWisU+z0LdgwQJEIhG8+uqriEajeOONN+DxeLBw4cKk9y1atAjbtm3Dvn37EA6H
sXHjRlx++eVwOp19zsPhcKCqqgrPP/88gsEgPv74Y2zduhWLFy/ut299tUn6JcsSgpF4Rv6KNShy
+gZ9+tD96JXhu5SbqkS9n6JI/dY1jhWSJCEU1eYmnl5tA8P6+B4iGh002+tNJhNefvllvP3225g/
fz5ee+01vPTSS7DZbFi3bh3WrVsHAJg2bRoee+wxPPzww1iwYAGam5vx4x//uN95AMBjjz2GWCyG
K664AmvWrMHatWsxa9asfvvWV5ukb2N3rGlk4mUiIqL0kQSfMDkodXV1qKqqwvbt21FWVpbp7lCa
nGrqQodPu+fpJZhNCipLs6HI2o/G8NcGtNPcHkRTW0DzdmUJmD4hj98zjUiJ8+u6Z19Bvruoz/fy
Ro6B4fg+ERERkQ4w9BERERHpAEMfESXhJT8iorGJoY+IiIhIBxj6iCgJ7+0iIhqbGPqIiIiIdICh
j4iIiEgHGPqIiIiIdIChj4iIiEgHGPqIiIiIdIChj4iS8Dl9RERj05BDXzgcHo5+EBEREVEaGVJ5
U3t7O37605/i6NGjiMfjALqf5RWNRnHs2DHs27cvrZ0kIu0IITjaR0Q0BqU00rd+/Xps3boVhYWF
2LdvH0pKShCNRnHgwAGsXLky3X0k4gODdSJT3zO3LyLSg5RG+nbt2oWNGzfiy1/+Mg4ePIjbbrsN
06ZNw4YNG3D48OF095F0TlUFBAQkAciytiNQDqsRXn8EWmeCcCSOaEyFUACDol3pbTyuQj0z0qfI
kmYjfkIIxNXulSxJAoqs3TLH4mrP6KaW61oIAYtJ0ay9pLYBRKIqzBlqn4gyI6XQFwwGMWnSJADA
hAkTcOjQIUybNg233nor7rrrrrR2kPRLVQVUIVDf4kenP4JchwnFLjtkSdIs/DmzLbBZjKj3+BAI
xdIe/lRVIBZXUdfchUM1Hkwe58RFlS7IcnqXWRUCqipw+HgbDte2Icdhxt/MKITDZkp7EIrFVXR0
hbHnUCP8wRgumpSPSWW5af+e46oKVRX46EgLauq9KHLZMX96IUxGJa3LLISAEEBXMILTLYG0tXMu
kgQosoQSlx0mI+/jI9KblEJfaWkpampqUFxcjAkTJvSM7imKgs7OzrR2kPQncVJsbg/A4w31BK0O
XwRefwQFTitcOVZIkjZ3mppNCiaW5MAXiKKuxXdmZGh42xBnQldjqx/tXZ/fHFV9oh21DZ2YPcWN
cYVZwz76lhhhO+3xY391M4LhGACgrTOE/9p1AuOLsjB3agEMigxlmINQLK4iElWx93AjGlr8PdM/
OtKCoyc7cMm0Qrid1mEPYImAW1PvxcfHPIjGVADAaY8ff3i/BheU52LmJDdkuXu0c1jbVgUisTjq
m/0InFnXWpEkdO87uVbIrNmkYcQ64NEjpdB3ww03YO3atXjqqadw1VVX4Y477kBZWRl27NiBKVOm
pLuPpBOJsNcZiOC0x49YvHeyEgJoaguirTOMYpcdWVbjsIS/RE1XX/Nx2IyYMi4XbZ0hNLYGIET3
ZbKhEVDV7pDV3BaAeo40GY7G8cEnjThyvB3zZxQix2EeliAUi6vwBSL44GAT2jpD53zPicYu1DX7
MH1iHqaOz+secRziulbV7tD1f595cPRkO9RzrER/MIq/fFiHAqcV82cUwWo2DNsyezqC2Hu4Cb5A
tNfrQgBHT3bg+OlOzJ7sxvii7OFbZtEdrjt8kSHNa6AkCcixm1CUb4fRwNE9Gl6shx1dUgp999xz
DywWC1RVxezZs/Gd73wHL774IoqLi/HMM8+ku4+kA6oqEI7GUdfsQygS7/f90ZiKk41dsJkNKC2w
w2RQhnQpMNXQKEkS8nOsyHWY0dgWQHtXeNCjfqoqEAhF0dDiQ+TMaFNfOnxhbPvgJErdDlwyvRAm
w+BG3+JxFbG4wP7qJpxo7Or//arA/x1rxbFTXsydWoBil31QI46JUcVTTV346EgLwtH+v+fm9iDe
3lGLiaU5mD3ZDUWRBlXvF4urCEVi2HOwCU1t/V9SjURV7DnYhOrj7fibGUXIzRpc0E78IePxBtHc
HtS0NlSSALNRQVmBA1ZzSod6ogHjCN/oIokUYvrevXsxe/ZsGI3GpOmRSATvvvsurr766rR1cKSq
q6tDVVUVtm/fjrKyskx3Z9RKjIA0tPjh9Q9+BCQT9X5A9w0XA633S9Tt1Tf74A/1Hm1KhSxJmDI+
FxdWulJe5sRlzeoTbThU09Zz48RA5edYMH9GERxWY8pBKBZX4fWFsedgEzp8g3u2p9Eg46LKfFQO
oN5PVVXEVYEDR1vwWb130KGrxGXvDtoDqPdTVQFfMIoGj7/nErIWJKn7hqdSlx3ZdhNPyjTqJM6v
6559Bfnuoj7fe92CCm06NUak9Offt771Lfzv//4v8vLykqbX19fj+9//Pj7++OO0dI7SL1O1GIkR
kJaOIFo6hj4C0uGLoNMfgTtD9X5dgQjqW/x91vudr25vMFQhcPh4O2oaOjFnshvlfdT7JUbYGlv9
2Hf487q9wWr1hvBfO4+fqfcrhEGRzjviGI+riMRU7D3UhPoW35DajcZUfJio95teCFfu+ev9Estc
W+/FX8+q2xusBo8fb71fg8njnLiw0gWljxtrMl2353Za4c6xDvsfP6zb+lwq5SBEI9F5Q99vfvMb
/PznPwfQvYHfdNNNkL9wWaWzsxMTJkxIbw8pbTJRi9Fz52Iggobz1O0NlnpWvV+Jyw6H1ajZqF+W
zYQp44xo9YbQ1Na73k9VBdo7Q2hqD0Ad5AjbuYQjcez+pBHV56n3i8VV+INRfHCwEa3ec9ftDdaJ
xi7UtfgwY0I+pox3JtW+JUZwD37mQfWJjnPWKg6WLxjF/+yvQ2GeDfNnFMJiMvRa5lZvEHsPNaHr
HHV7g6WK7htrEkF7XFFy0M503V62rXu0O511ewx+n+N6oNHovKFvyZIl6OzshKqqePHFF7Fo0SLY
bLae1yVJgt1uxzXXXKNJR2n4aXnQSoS9cDSO+hYfguH+67kGKxpTcaKxCzaLAWVuO0zG7meRpXt5
JUmCK9cKZ9aZer/OMOIDrNsbrES9X1mBA/OmFcJokBEfQN3eYMXjAh8f8+DYqQ7MnVaAonw7AOBU
UxcOHG1JqT5zsJraAtj6fi0mluVgzmQ3JElCOBLHnkONaGxN36NQItE4PjjYiOoTbZg/vQjOLDMk
SYLHG0RLe/CcN6aki5Z1eww5n+O6oNHqvEcJi8XS82sbxcXFuP7662EymTTrGI0twVAMzR3BYR15
6U8gFMPRU15MHZ8Lo0G7h9AqioxStwOBYBTVJ9rhC2q3zHXNPjS0+FHssqGxLYD4MI6k9iUQjuH9
Aw1wZpmhCgGvRiNdAsBndV6cbOyCO9eK061+zW6W8Poi+POek5h1QffjXbSs2wMAq0WBO9eGbJuR
IYSIUnLe0PfWW2/h2muvhclkgsFgwH//93+fdyaLFy9OS+do7BDAoG9aGHLbGXqigCxLQ66fG4zE
A60zYai1ioMVjalo8GRmmTv9Editxv7fOMxkSYLDamDgI137067jPf/Pmzr6d97Qt3btWlx22WXI
z8/H2rVrzzsDSZIY+oiIiIhGuPOGvurq6nP+PxERERGNPgOq/D1+/DiOHj0KWZYxffp0lJSUpKtf
RERERDSMUgp9XV1duO+++7Bjx46eaZIk4dprr8XTTz8Ns9mctg4SERER0dCl9ECnDRs2oL6+Hr/6
1a9w4MAB7N+/H5s2bcLhw4fx7LPPpruPRERERDREKYW+//mf/8ETTzyBSy+9FBaLBXa7HV/60pfw
+OOPY+vWrenuIxERERENUUqhz2KxwGDofSU4Kytr2DtERERERMMvpdB3zz33YN26dTh27FjPtMbG
Rjz55JP47ne/m7bOEREREdHwSOlGjldeeQUNDQ1YvHgxsrOzYTQa0dbWBlVV8eGHH+KZZ57pee8n
n3ySts4SERER0eCkFPruueeedPeDiIiIiNIopdB34403prsfRERERJRGKYW+cDiM3/72tzh69Cji
8XjP9Egkgk8++aTP3+UlIiIiosxLKfQ9+uijePvttzFz5kzs378f8+bNw6lTp9DY2Ig77rgj3X0k
IiIioiFK+Tl9Tz31FF599VWUl5dj/fr1eOedd3DNNdcgEAiku49ERERENEQphb6uri7MmjULADBp
0iR88sknUBQFK1aswHvvvZfWDtLYIctSRtqNx1UIITRvVwiBDC0yTEYlI+0qigQlQwudqXb1KJah
fYqIhialy7sFBQVoampCSUkJKioqcOTIEQDdD2dua2tLawdpbLBZDJg6zolWbwhN7UGoqnYnjGP1
nTAbFZQV2GGzGNPenhACnf4IQlEVF4zLQ0t7AJ6OILRYYrvFgHnTClHssqO5PYi9hxrRFYimvV1J
AiaV5WLWBS4IAXx4pBm1DZ1pbxcALCYFpW4HLGYDOv0RnPb4EIunf21LElDgtMFmSekwOqzsVgPK
3A7IkrZBNxZXcbrVj46uCMwmBWVubfYpIhoeKR2trr76avzwhz/EU089hcsuuwxr167FxRdfjO3b
t6O8vDzdfaQ0EUJA0uikkWgnL9sCZ7YZja0BtHWGNWkbAMLROGoaOpFlNaLYbYfJkJ6RsGA4hrpm
HyLROIToHt1059mQn2NFvceHLn8kLe0aFAkXTszHBeOckCUJkiShwGnFdQsqUNvgxV8/9SAaU9PS
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
FgAAAABJRU5ErkJggg==
)</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

## Discussion based on the research question[¶](#Discussion-based-on-the-research-question)

The visualization on the relationships between crime rate and number of religion adherents are shown here. The data were obtained from FBI data and religion census. Both data sets are for the year of 2010\. 1) the total numbers of crime and 2) the total number of murder cases per population of each metropolitan area were calculated and used in the comparison. The first figure shows the scatter plot of all-religions adherents per capita versus all crime cases. It can be seen that the scatter plot itself does not show a clear relationship between these two parameters. Therefore, the number of people who have a certain religion does not directly correlate to the number of crime or murder case that a certain city will face. Ann Arbor MI, in particular has the number of religions adherents per capita of 0.328 (below average) and have murder and crime per capita of 0.000014 (below average) and 0.0305 (about the average), respectively. Ann Arbor can be considered as a low religions adherent city with below average murder cases and about average in terms of overall crime cases.

Therefore, the number of people who have a certain religion does not correlate to the amount of crime. It should be noted that the histogram above x-axis, is just about how the religions data are distributed regardless of crime/murder cases. In most cities, less than 70% of people have religion. This analysis shows that the quantity of religion adherents did not (in 2010) help to reduce the crime or murder cases. It was not correlated with the crime/murder cases. This is somewhat contradict to the belief that as more people have religion, people will be better and the crime should reduce. Yet, this does not necessarily counter intuitive, because the quantity does not tell the quality or how religious of a person is. Based on the obtained census data, when one person stated that s/he is a religion adherent, it does not mean that s/he won't commit a crime.

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Discussion based on Cairo's principle[¶](#Discussion-based-on-Cairo's-principle)

#### Truthfulness, beauty, functionality, and insightfulness[¶](#Truthfulness,-beauty,-functionality,-and-insightfulness)

My design choice for my visual in regards to truthfulness is to use seaborn joint point. This plot give the true value of data and the histogram of each axis. The color of Ann Arbor is designated to be red. This information is described in the legend part. The data were cleaned completely and it was adjusted to be in the form of value per the number of population. This is to allow a fair comparison between the city with small and large population.

In regard to Cairo's principle of beauty, I minimize the non-data-ink, and enlarge the font size of the axis label and ticks label so that it is legible for the reader. The hex plots were also provided so that they can beautifully give the information on the density of the data. Red color for Ann Arbor was used so that it was clear on the location of the Ann Arbor data.

In regard to Cairo's principle of functionality, my visualization give the full functionality to the reader to understand the relationship between the number of religion adherents and the number of crime and murder cases per capita. The histogram allow the reader to see the comparison between Ann Arbor and the rest of US clearly.

In regard to Cairo's principle of insightfulness, my visualization tell the answer to the important research question. This answer tell whether the number of religion adherent can help to reduce the crime or not. Intuitively, the number of people who has religions should commit less crime compared to people who are not abide by a certain religious belief. Nevertheless, my graph show that the quantity or number of people who have religion alone does not help to reduce the crime. The data suggests that the concentration degree of a person who is abide by religion's rule should also be considered. Therefore, this visualization help to not casually conclude that the number of religion adherent can indicate the decrease in crime or murder cases.

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

## Appendix: Intermediate data values[¶](#Appendix:-Intermediate-data-values)

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [8]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="n">dfm</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[8]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div>

<table border="1" class="dataframe">

<thead>

<tr style="text-align: right;">

<th>ADH per capita</th>

<th>Murder per capita</th>

<th>Crime per capita</th>

</tr>

<tr>

<th>Metro name</th>

</tr>

</thead>

<tbody>

<tr>

<th>Abilene, TX</th>

<td>0.632222</td>

<td>0.000031</td>

<td>0.040403</td>

</tr>

<tr>

<th>Akron, OH</th>

<td>0.425786</td>

<td>0.000037</td>

<td>0.034903</td>

</tr>

<tr>

<th>Albany, GA</th>

<td>0.516782</td>

<td>0.000087</td>

<td>0.050786</td>

</tr>

<tr>

<th>Albany-Schenectady-Troy, NY</th>

<td>0.428175</td>

<td>0.000015</td>

<td>0.030041</td>

</tr>

<tr>

<th>Albuquerque, NM</th>

<td>0.460034</td>

<td>0.000058</td>

<td>0.045666</td>

</tr>

<tr>

<th>Alexandria, LA</th>

<td>0.670879</td>

<td>0.000058</td>

<td>0.052308</td>

</tr>

<tr>

<th>Allentown-Bethlehem-Easton, PA-NJ</th>

<td>0.500896</td>

<td>0.000035</td>

<td>0.025262</td>

</tr>

<tr>

<th>Altoona, PA</th>

<td>0.524302</td>

<td>0.000008</td>

<td>0.020552</td>

</tr>

<tr>

<th>Amarillo, TX</th>

<td>0.682457</td>

<td>0.000057</td>

<td>0.053257</td>

</tr>

<tr>

<th>Ames, IA</th>

<td>0.490954</td>

<td>0.000012</td>

<td>0.031841</td>

</tr>

<tr>

<th>Anchorage, AK</th>

<td>0.339950</td>

<td>0.000042</td>

<td>0.043192</td>

</tr>

<tr>

<th>Anderson, IN</th>

<td>0.369382</td>

<td>0.000023</td>

<td>0.035595</td>

</tr>

<tr>

<th>Anderson, SC</th>

<td>0.649621</td>

<td>0.000053</td>

<td>0.052938</td>

</tr>

<tr>

<th>Ann Arbor, MI</th>

<td>0.328303</td>

<td>0.000014</td>

<td>0.030521</td>

</tr>

<tr>

<th>Appleton, WI</th>

<td>0.672817</td>

<td>0.000000</td>

<td>0.022925</td>

</tr>

<tr>

<th>Asheville, NC</th>

<td>0.543191</td>

<td>0.000019</td>

<td>0.026846</td>

</tr>

<tr>

<th>Athens-Clarke County, GA</th>

<td>0.395931</td>

<td>0.000042</td>

<td>0.042185</td>

</tr>

<tr>

<th>Atlanta-Sandy Springs-Marietta, GA</th>

<td>0.497212</td>

<td>0.000061</td>

<td>0.038764</td>

</tr>

<tr>

<th>Atlantic City-Hammonton, NJ</th>

<td>0.426842</td>

<td>0.000080</td>

<td>0.040802</td>

</tr>

<tr>

<th>Augusta-Richmond County, GA-SC</th>

<td>0.529106</td>

<td>0.000102</td>

<td>0.052282</td>

</tr>

<tr>

<th>Austin-Round Rock-San Marcos, TX</th>

<td>0.439207</td>

<td>0.000034</td>

<td>0.041199</td>

</tr>

<tr>

<th>Bakersfield-Delano, CA</th>

<td>0.480190</td>

<td>0.000090</td>

<td>0.043062</td>

</tr>

<tr>

<th>Baltimore-Towson, MD</th>

<td>0.420772</td>

<td>0.000103</td>

<td>0.037760</td>

</tr>

<tr>

<th>Bangor, ME</th>

<td>0.240737</td>

<td>0.000020</td>

<td>0.031667</td>

</tr>

<tr>

<th>Barnstable Town, MA</th>

<td>0.534351</td>

<td>0.000005</td>

<td>0.034074</td>

</tr>

<tr>

<th>Battle Creek, MI</th>

<td>0.326040</td>

<td>0.000045</td>

<td>0.044012</td>

</tr>

<tr>

<th>Bay City, MI</th>

<td>0.534680</td>

<td>0.000009</td>

<td>0.028075</td>

</tr>

<tr>

<th>Beaumont-Port Arthur, TX</th>

<td>0.712385</td>

<td>0.000056</td>

<td>0.043636</td>

</tr>

<tr>

<th>Bellingham, WA</th>

<td>0.289261</td>

<td>0.000025</td>

<td>0.034647</td>

</tr>

<tr>

<th>Billings, MT</th>

<td>0.392648</td>

<td>0.000019</td>

<td>0.040209</td>

</tr>

<tr>

<th>...</th>

<td>...</td>

<td>...</td>

<td>...</td>

</tr>

<tr>

<th>Texarkana, TX-Texarkana, AR</th>

<td>0.609725</td>

<td>0.000029</td>

<td>0.048907</td>

</tr>

<tr>

<th>Topeka, KS</th>

<td>0.457938</td>

<td>0.000060</td>

<td>0.046345</td>

</tr>

<tr>

<th>Trenton-Ewing, NJ</th>

<td>0.525992</td>

<td>0.000054</td>

<td>0.025278</td>

</tr>

<tr>

<th>Tulsa, OK</th>

<td>0.555646</td>

<td>0.000064</td>

<td>0.040476</td>

</tr>

<tr>

<th>Tuscaloosa, AL</th>

<td>0.529803</td>

<td>0.000051</td>

<td>0.044492</td>

</tr>

<tr>

<th>Tyler, TX</th>

<td>0.710053</td>

<td>0.000063</td>

<td>0.041207</td>

</tr>

<tr>

<th>Utica-Rome, NY</th>

<td>0.482550</td>

<td>0.000024</td>

<td>0.026470</td>

</tr>

<tr>

<th>Valdosta, GA</th>

<td>0.499019</td>

<td>0.000038</td>

<td>0.039777</td>

</tr>

<tr>

<th>Victoria, TX</th>

<td>0.709353</td>

<td>0.000069</td>

<td>0.044199</td>

</tr>

<tr>

<th>Vineland-Millville-Bridgeton, NJ</th>

<td>0.349985</td>

<td>0.000063</td>

<td>0.037916</td>

</tr>

<tr>

<th>Virginia Beach-Norfolk-Newport News, VA-NC</th>

<td>0.404169</td>

<td>0.000066</td>

<td>0.038195</td>

</tr>

<tr>

<th>Visalia-Porterville, CA</th>

<td>0.407385</td>

<td>0.000080</td>

<td>0.040904</td>

</tr>

<tr>

<th>Waco, TX</th>

<td>0.614105</td>

<td>0.000038</td>

<td>0.046397</td>

</tr>

<tr>

<th>Warner Robins, GA</th>

<td>0.527741</td>

<td>0.000060</td>

<td>0.041148</td>

</tr>

<tr>

<th>Washington-Arlington-Alexandria, DC-VA-MD-WV</th>

<td>0.445763</td>

<td>0.000053</td>

<td>0.029306</td>

</tr>

<tr>

<th>Waterloo-Cedar Falls, IA</th>

<td>0.570674</td>

<td>0.000024</td>

<td>0.026935</td>

</tr>

<tr>

<th>Wausau, WI</th>

<td>0.648658</td>

<td>0.000000</td>

<td>0.019205</td>

</tr>

<tr>

<th>Wenatchee-East Wenatchee, WA</th>

<td>0.441840</td>

<td>0.000009</td>

<td>0.028643</td>

</tr>

<tr>

<th>Wichita Falls, TX</th>

<td>0.663034</td>

<td>0.000048</td>

<td>0.045502</td>

</tr>

<tr>

<th>Wichita, KS</th>

<td>0.508087</td>

<td>0.000029</td>

<td>0.043809</td>

</tr>

<tr>

<th>Williamsport, PA</th>

<td>0.489764</td>

<td>0.000017</td>

<td>0.021569</td>

</tr>

<tr>

<th>Wilmington, NC</th>

<td>0.422420</td>

<td>0.000030</td>

<td>0.041074</td>

</tr>

<tr>

<th>Winchester, VA-WV</th>

<td>0.428646</td>

<td>0.000016</td>

<td>0.024757</td>

</tr>

<tr>

<th>Winston-Salem, NC</th>

<td>0.522328</td>

<td>0.000035</td>

<td>0.046436</td>

</tr>

<tr>

<th>Worcester, MA</th>

<td>0.519847</td>

<td>0.000017</td>

<td>0.026224</td>

</tr>

<tr>

<th>Yakima, WA</th>

<td>0.444828</td>

<td>0.000100</td>

<td>0.049855</td>

</tr>

<tr>

<th>York-Hanover, PA</th>

<td>0.388443</td>

<td>0.000025</td>

<td>0.023013</td>

</tr>

<tr>

<th>Youngstown-Warren-Boardman, OH-PA</th>

<td>0.529744</td>

<td>0.000059</td>

<td>0.036449</td>

</tr>

<tr>

<th>Yuba City, CA</th>

<td>0.319890</td>

<td>0.000042</td>

<td>0.028425</td>

</tr>

<tr>

<th>Yuma, AZ</th>

<td>0.373975</td>

<td>0.000047</td>

<td>0.030065</td>

</tr>

</tbody>

</table>

296 rows Ã— 3 columns

</div>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [9]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span><span class="n">dfm_mur</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[9]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div>

<table border="1" class="dataframe">

<thead>

<tr style="text-align: right;">

<th>ADH per capita</th>

<th>Murder per capita</th>

<th>Crime per capita</th>

</tr>

<tr>

<th>Metro name</th>

</tr>

</thead>

<tbody>

<tr>

<th>Abilene, TX</th>

<td>0.632222</td>

<td>0.000031</td>

<td>0.040403</td>

</tr>

<tr>

<th>Akron, OH</th>

<td>0.425786</td>

<td>0.000037</td>

<td>0.034903</td>

</tr>

<tr>

<th>Albany, GA</th>

<td>0.516782</td>

<td>0.000087</td>

<td>0.050786</td>

</tr>

<tr>

<th>Albany-Schenectady-Troy, NY</th>

<td>0.428175</td>

<td>0.000015</td>

<td>0.030041</td>

</tr>

<tr>

<th>Albuquerque, NM</th>

<td>0.460034</td>

<td>0.000058</td>

<td>0.045666</td>

</tr>

<tr>

<th>Alexandria, LA</th>

<td>0.670879</td>

<td>0.000058</td>

<td>0.052308</td>

</tr>

<tr>

<th>Allentown-Bethlehem-Easton, PA-NJ</th>

<td>0.500896</td>

<td>0.000035</td>

<td>0.025262</td>

</tr>

<tr>

<th>Altoona, PA</th>

<td>0.524302</td>

<td>0.000008</td>

<td>0.020552</td>

</tr>

<tr>

<th>Amarillo, TX</th>

<td>0.682457</td>

<td>0.000057</td>

<td>0.053257</td>

</tr>

<tr>

<th>Ames, IA</th>

<td>0.490954</td>

<td>0.000012</td>

<td>0.031841</td>

</tr>

<tr>

<th>Anchorage, AK</th>

<td>0.339950</td>

<td>0.000042</td>

<td>0.043192</td>

</tr>

<tr>

<th>Anderson, IN</th>

<td>0.369382</td>

<td>0.000023</td>

<td>0.035595</td>

</tr>

<tr>

<th>Anderson, SC</th>

<td>0.649621</td>

<td>0.000053</td>

<td>0.052938</td>

</tr>

<tr>

<th>Ann Arbor, MI</th>

<td>0.328303</td>

<td>0.000014</td>

<td>0.030521</td>

</tr>

<tr>

<th>Asheville, NC</th>

<td>0.543191</td>

<td>0.000019</td>

<td>0.026846</td>

</tr>

<tr>

<th>Athens-Clarke County, GA</th>

<td>0.395931</td>

<td>0.000042</td>

<td>0.042185</td>

</tr>

<tr>

<th>Atlanta-Sandy Springs-Marietta, GA</th>

<td>0.497212</td>

<td>0.000061</td>

<td>0.038764</td>

</tr>

<tr>

<th>Atlantic City-Hammonton, NJ</th>

<td>0.426842</td>

<td>0.000080</td>

<td>0.040802</td>

</tr>

<tr>

<th>Augusta-Richmond County, GA-SC</th>

<td>0.529106</td>

<td>0.000102</td>

<td>0.052282</td>

</tr>

<tr>

<th>Austin-Round Rock-San Marcos, TX</th>

<td>0.439207</td>

<td>0.000034</td>

<td>0.041199</td>

</tr>

<tr>

<th>Bakersfield-Delano, CA</th>

<td>0.480190</td>

<td>0.000090</td>

<td>0.043062</td>

</tr>

<tr>

<th>Baltimore-Towson, MD</th>

<td>0.420772</td>

<td>0.000103</td>

<td>0.037760</td>

</tr>

<tr>

<th>Bangor, ME</th>

<td>0.240737</td>

<td>0.000020</td>

<td>0.031667</td>

</tr>

<tr>

<th>Barnstable Town, MA</th>

<td>0.534351</td>

<td>0.000005</td>

<td>0.034074</td>

</tr>

<tr>

<th>Battle Creek, MI</th>

<td>0.326040</td>

<td>0.000045</td>

<td>0.044012</td>

</tr>

<tr>

<th>Bay City, MI</th>

<td>0.534680</td>

<td>0.000009</td>

<td>0.028075</td>

</tr>

<tr>

<th>Beaumont-Port Arthur, TX</th>

<td>0.712385</td>

<td>0.000056</td>

<td>0.043636</td>

</tr>

<tr>

<th>Bellingham, WA</th>

<td>0.289261</td>

<td>0.000025</td>

<td>0.034647</td>

</tr>

<tr>

<th>Billings, MT</th>

<td>0.392648</td>

<td>0.000019</td>

<td>0.040209</td>

</tr>

<tr>

<th>Binghamton, NY</th>

<td>0.480878</td>

<td>0.000025</td>

<td>0.027281</td>

</tr>

<tr>

<th>...</th>

<td>...</td>

<td>...</td>

<td>...</td>

</tr>

<tr>

<th>Tampa-St. Petersburg-Clearwater, FL</th>

<td>0.347609</td>

<td>0.000043</td>

<td>0.038872</td>

</tr>

<tr>

<th>Texarkana, TX-Texarkana, AR</th>

<td>0.609725</td>

<td>0.000029</td>

<td>0.048907</td>

</tr>

<tr>

<th>Topeka, KS</th>

<td>0.457938</td>

<td>0.000060</td>

<td>0.046345</td>

</tr>

<tr>

<th>Trenton-Ewing, NJ</th>

<td>0.525992</td>

<td>0.000054</td>

<td>0.025278</td>

</tr>

<tr>

<th>Tulsa, OK</th>

<td>0.555646</td>

<td>0.000064</td>

<td>0.040476</td>

</tr>

<tr>

<th>Tuscaloosa, AL</th>

<td>0.529803</td>

<td>0.000051</td>

<td>0.044492</td>

</tr>

<tr>

<th>Tyler, TX</th>

<td>0.710053</td>

<td>0.000063</td>

<td>0.041207</td>

</tr>

<tr>

<th>Utica-Rome, NY</th>

<td>0.482550</td>

<td>0.000024</td>

<td>0.026470</td>

</tr>

<tr>

<th>Valdosta, GA</th>

<td>0.499019</td>

<td>0.000038</td>

<td>0.039777</td>

</tr>

<tr>

<th>Victoria, TX</th>

<td>0.709353</td>

<td>0.000069</td>

<td>0.044199</td>

</tr>

<tr>

<th>Vineland-Millville-Bridgeton, NJ</th>

<td>0.349985</td>

<td>0.000063</td>

<td>0.037916</td>

</tr>

<tr>

<th>Virginia Beach-Norfolk-Newport News, VA-NC</th>

<td>0.404169</td>

<td>0.000066</td>

<td>0.038195</td>

</tr>

<tr>

<th>Visalia-Porterville, CA</th>

<td>0.407385</td>

<td>0.000080</td>

<td>0.040904</td>

</tr>

<tr>

<th>Waco, TX</th>

<td>0.614105</td>

<td>0.000038</td>

<td>0.046397</td>

</tr>

<tr>

<th>Warner Robins, GA</th>

<td>0.527741</td>

<td>0.000060</td>

<td>0.041148</td>

</tr>

<tr>

<th>Washington-Arlington-Alexandria, DC-VA-MD-WV</th>

<td>0.445763</td>

<td>0.000053</td>

<td>0.029306</td>

</tr>

<tr>

<th>Waterloo-Cedar Falls, IA</th>

<td>0.570674</td>

<td>0.000024</td>

<td>0.026935</td>

</tr>

<tr>

<th>Wenatchee-East Wenatchee, WA</th>

<td>0.441840</td>

<td>0.000009</td>

<td>0.028643</td>

</tr>

<tr>

<th>Wichita Falls, TX</th>

<td>0.663034</td>

<td>0.000048</td>

<td>0.045502</td>

</tr>

<tr>

<th>Wichita, KS</th>

<td>0.508087</td>

<td>0.000029</td>

<td>0.043809</td>

</tr>

<tr>

<th>Williamsport, PA</th>

<td>0.489764</td>

<td>0.000017</td>

<td>0.021569</td>

</tr>

<tr>

<th>Wilmington, NC</th>

<td>0.422420</td>

<td>0.000030</td>

<td>0.041074</td>

</tr>

<tr>

<th>Winchester, VA-WV</th>

<td>0.428646</td>

<td>0.000016</td>

<td>0.024757</td>

</tr>

<tr>

<th>Winston-Salem, NC</th>

<td>0.522328</td>

<td>0.000035</td>

<td>0.046436</td>

</tr>

<tr>

<th>Worcester, MA</th>

<td>0.519847</td>

<td>0.000017</td>

<td>0.026224</td>

</tr>

<tr>

<th>Yakima, WA</th>

<td>0.444828</td>

<td>0.000100</td>

<td>0.049855</td>

</tr>

<tr>

<th>York-Hanover, PA</th>

<td>0.388443</td>

<td>0.000025</td>

<td>0.023013</td>

</tr>

<tr>

<th>Youngstown-Warren-Boardman, OH-PA</th>

<td>0.529744</td>

<td>0.000059</td>

<td>0.036449</td>

</tr>

<tr>

<th>Yuba City, CA</th>

<td>0.319890</td>

<td>0.000042</td>

<td>0.028425</td>

</tr>

<tr>

<th>Yuma, AZ</th>

<td>0.373975</td>

<td>0.000047</td>

<td>0.030065</td>

</tr>

</tbody>

</table>

274 rows Ã— 3 columns

</div>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [ ]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython3">

<pre><span></span> 
</pre>

</div>

</div>

</div>

</div>

</div>

</div>

</div>
