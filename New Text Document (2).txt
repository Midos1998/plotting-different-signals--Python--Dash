'''Includes'''
import dash
import dash_core_components as dcc
import dash_html_components as html
import numpy as np
import plotly
plotly.tools.set_credentials_file(username='andrew_wafeeq', api_key='y5erX7Rpwbp4Jj5sxdY1')
plotly.tools.set_config_file(world_readable=True, sharing='public')
import plotly.graph_objs as go
import pyaudio
import plotly
import scipy.io
#############################################################################################
'''Global Variables'''
class mat_arr(object):
    mat = []
class con_arr(object):
    h1 = []
class add_mat(object):
    path = ""
class time_arr(object):
    time = np.arange(0,10,.001) 
class dig_s(object):
    dig_1 = 0
    dig_2 = 0
    dig_3 = 0
class var_sig(object):
    freq = 0
    amp = 0
    sam_freq = 0
    sig_type = ""
class val_rad(object):
    rad = ""
class dig(object):
    f = 0
    duraion=0
matt = mat_arr.mat
colors = {
    'background': '#79FC79',
    'text': 'black'
}
#############################################################################################
def periodic_signal( freq ,amp , samples , func ):
    ts = np.arange( 0 , 0.3 , 1 / (samples * freq) )
    phases = 2 * np.pi * freq * ts 
    ys = amp * func( phases )
    return ts , ys
def sin_signal( freq , amp , samples):
    return periodic_signal(freq, amp, samples, np.sin )

def cos_signal( freq , amp , samples):
    return periodic_signal(freq, amp, samples, np.cos )

def exp_signal( freq , amp , samples):
    return periodic_signal( freq, amp, samples, np.exp )
#############################################################################################
app = dash.Dash()
app.config['suppress_callback_exceptions']=True
app.layout = html.Div([
        #'''TASK1'''
        html.Div([html.H1('Choose Your File', style={'color':'#C09010','font-weight':'bold'}),
        html.Div([
                dcc.Upload(id='my-input',
                children=html.Div([
                'Drag and Drop or ',
                html.Button('Select a File')
                ]), style={
                    'width': '98%',
                    'height': '60px',
                    'lineHeight': '60px',
                    'borderWidth': '1px',
                    'borderStyle': 'dashed',
                    'borderRadius': '5px',
                    'textAlign': 'center',
                    'margin':'10px'
                })
        ]),
        html.Div([html.Button('Plot', id='plot-button',style={'width':'120px','height':'50px','margin':'10px'}),
        dcc.Graph(
        id='my-graph1',
        figure={
             'layout': go.Layout(
                xaxis={'type': 'time', 'title': 'Time (msec)'},
                yaxis={'title': 'Volt(MicroVolt)'},
                #margin={'l': 40, 'b': 40, 't': 10, 'r': 10},
                legend={'x': 0, 'y': 1},
                hovermode='closest'
            )
        })]),
        html.Div(html.H1('Filer', style={'color':'#C09010','font-weight':'bold'})),
        html.H3('Enter Filter Parameter'),
        #html.Div(dcc.Input(id='input-1', type="number",style={'width':'70px','margin':'5px'})),
        dcc.Input(id='input-box11', type='number',style={'width':'60px','margin':'5px'}),
        dcc.Input(id='input-box12', type='number',style={'width':'60px','margin':'5px'}),
        dcc.Input(id='input-box13', type='number',style={'width':'60px','margin':'5px'}),
        html.Br(),
        html.Button('Set 1', id='button11',style={'width':'60px','margin':'5px'}),
        html.Button('Set 2', id='button12',style={'width':'60px','margin':'5px'}),
        html.Button('Set 3', id='button13',style={'width':'60px','margin':'5px'}),
        html.Div(id='output-container13',
        children='Enter three parameter and press submit'),
        html.Div(id='output-container11', children=''),
        html.Div(id='output-container12', children=''),
        #html.Div(id='output-container3', children=''),
        html.Div(html.Button('Plot', id='filter-button',style={'width':'120px','height':'50px','margin':'10px'})),
        dcc.Graph(
        id='my-graph2',
        figure={
             'layout': go.Layout(
                xaxis={'type': 'time', 'title': 'Time (msec)'},
                yaxis={'title': 'Volt(MicroVolt)'},
                #margin={'l': 40, 'b': 40, 't': 10, 'r': 10},
                legend={'x': 0, 'y': 1},
                hovermode='closest'
            )
        }),html.Hr(),
#############################################################################################
        #Task2
        html.H1('Plot Your Signal!',style={'color':'#C09010','font-weight':'bold'}),
        html.Div(dcc.Dropdown(
                id='signal-type',
        options=[
            {'label': 'Exponential', 'value': 'ex'},
            {'label': 'Sinsoidal', 'value': 'sin'}
        ],
        value='ex'
        ),style={'width':'360px','margin':'10px auto'}),
#############################################################################################
        html.Div(id='output-container1', children=''),
        html.Div(id='output-container2', children=''),
        html.Div(id='output-container3', children=''),
        html.Div(id='output-container4', children=''),
        html.Div([html.P('Frequency'),
                dcc.Input(id='input-box1', type='number',
                style={'width':'110px','height':'25px','margin':'5px'}),
                html.Button('Set', id='button1',style={'width':'110px','height':'25px','margin':'5px'})]),
        html.Div([html.P('Amplitude'),
                dcc.Input(id='input-box2', type='number',
                style={'width':'110px','height':'25px','margin':'5px'}),
                html.Button('Set', id='button2',style={'width':'110px','height':'25px','margin':'5px'})]),
        html.Div([html.P('Frequency Sample'),
                dcc.Input(id='input-box3', type='number',
                style={'width':'110px','height':'25px','margin':'5px'}),
                html.Button('Set', id='button3',style={'width':'110px','height':'25px','margin':'5px'})]),
        html.Button('Plot', id='button',style={'width':'120px','height':'50px','margin':'10px'}),
        dcc.Graph(
        id='my-graph',
        figure={
             'layout': go.Layout(
                                #xaxis={'type': 'time', 'title': 'Time (msec)'},
                                #yaxis={'title': 'Volt(MicroVolt)'},
                                #margin={'l': 40, 'b': 40, 't': 10, 'r': 10},
                                legend={'x': 0, 'y': 1},
                                hovermode='closest'
                                )
                }),html.Hr(),
#############################################################################################
        #Task2

        html.H1(children='Hear Your Signal!',
        style={'color':'#C09010','font-weight':'bold'}),
               
    # In[4]:Duraion of signal                    
        html.Div([html.H3(
        children='Select Duraion of Sound',
        style={
                'textAlign': 'center',
                'backgroundColor':  '#959BC6'
            
            }
        ),
        dcc.RadioItems(id='Duraion of signal',
            options=[
                {'label': '3 SECOND', 'value': '1'},
                {'label': '5 SECOND', 'value': '2'},
                {'label': '10 SECOND', 'value': '3'},
                {'label': '20 SECOND', 'value': '4'},

                    ],style={'font-size':'15px',
                        	'font-weight':'bold',
                        	'padding':'13px 76px'}
         ),html.Div(id='output-container32',children='0000'),html.Hr()]),

# In[5]:type of FREQ 
        html.Div([html.H3(
        children='Select Frequency',
        style={
            'textAlign': 'center',
            'backgroundColor':  '#959BC6'
            
        }
        ),
           dcc.RadioItems(id='type of Freq',
                options=[
                     
                    {'label': '50 Hz', 'value': '1'},
                  
                    {'label': '250 Hz', 'value': '2'},
                 
                    {'label': '350 Hz', 'value': '3'},
                   
                    {'label': '440 Hz', 'value': '4'},
                             
                        ],
                style={'font-size':'15px',
                    	'font-weight':'bold',
                    	'padding':'13px 76px'}
                
             ),html.Div(id='output-container33',children='0000'),html.Hr()]),
                   
# In[3]:type of signal
    
        html.Div([html.H3(
        children='Type of Signals',
        style={
            'textAlign': 'center',
            'backgroundColor':  '#959BC6'
            
        }
        ),
           dcc.RadioItems(id='type of signal',
                options=[
                    {'label': 'one of sinosoidal signal','value': '1'},
                    {'label': 'mixture of 2 sinosoidal signal', 'value': '2'},
                    {'label': 'mixture of 3 sinosoidal signal', 'value': '3'},
                    {'label': 'mixture of 1 sinosoid and exponential signal', 'value': '4'},
                    {'label': 'mixture of 2 sinosoid and exponential signal','value': '5'},

                        ],
                style={'font-size':'15px',
                    	'font-weight':'bold',
                    	'padding':'13px 76px'}
             ),html.Div(id='output-container31',children='0000'),html.Hr()]),


# In[6]:Button To apply
        html.Button('Submit', id='button31',style={'width':'110px','height':'25px','margin':'5px'})
        ],style={'margin':'10px auto','border':'1px solid #2B458A','background-color':'#F0EDED','text-align':'center','width':'1010px'})
        ],style={'background-color':'#052744'})

#'''CallBackTAsk1
#############################################################################################   
@app.callback(dash.dependencies.Output('my-graph1', 'figure'),
              [dash.dependencies.Input('plot-button','n_clicks'),dash.dependencies.Input('my-input', 'filename')])
def update_graph1(n_clicks,filename):
    add_mat.path = filename
    mat_arr.mat = scipy.io.loadmat(add_mat.path)['val']
    return {
        'data': [{
                    'x': time_arr.time,
                    'y': mat_arr.mat[0],
                    'type': 'scatter',
                    'name': 'SF'
                }]
    }
############################################################################################# 
@app.callback(
        dash.dependencies.Output('my-graph2', 'figure'),
        [dash.dependencies.Input('filter-button','n_clicks')])
def update_graph2(n_clicks):
    con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
    matcop = mat_arr.mat
    matcop = np.convolve(matcop[0], con_arr.h1)
    return {
        'data': [{
                    'x': time_arr.time,
                    'y': matcop,
                    'type': 'scatter',
                    'name': 'SF'
                }]
            }
#############################################################################################
@app.callback(
    dash.dependencies.Output('output-container11', 'children'),
    [dash.dependencies.Input('button11', 'n_clicks')],
    [dash.dependencies.State('input-box11', 'value')])      
def update_output1(n_clicks,value):
    value = int(value)
    if ((value/10) < 1) and ((value/10) > -1):
        dig_s.dig_1 = value
        con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
        return
    else:
        dig_s.dig_1 = value % 10
        con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
        return
 #############################################################################################       
@app.callback(
    dash.dependencies.Output('output-container12', 'children'),
    [dash.dependencies.Input('button12', 'n_clicks')],
    [dash.dependencies.State('input-box12', 'value')])      
def update_output2(n_clicks,value):
    value = int(value)
    if ((value/10) < 1) and ((value/10) > -1):
        dig_s.dig_2 = value
        con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
        return
    else:
        dig_s.dig_2 = value % 10
        con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
        return       
#############################################################################################
@app.callback(
    dash.dependencies.Output('output-container13', 'children'),
    [dash.dependencies.Input('button13', 'n_clicks')],
    [dash.dependencies.State('input-box13', 'value')])      
def update_output3(n_clicks,value):
    value = int(value)
    if ((value/10) < 1) and ((value/10) > -1):
        dig_s.dig_3 = value
        con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
    else:
        dig_s.dig_3 = value % 10
        con_arr.h1 = [dig_s.dig_1,dig_s.dig_2,dig_s.dig_3]
    return 'The input parameter was "{} {} {}"'.format(
        dig_s.dig_1,dig_s.dig_2,dig_s.dig_3
    )
#'''CallBackTAsk2
#############################################################################################
@app.callback(
    dash.dependencies.Output('output-container1', 'children'),
    [dash.dependencies.Input('button1', 'n_clicks')],
    [dash.dependencies.State('input-box1', 'value')])      
def update_output21(n_clicks,value):
    value = int(value)
    var_sig.freq = value
    return 
#############################################################################################
@app.callback(
    dash.dependencies.Output('output-container2', 'children'),
    [dash.dependencies.Input('button2', 'n_clicks')],
    [dash.dependencies.State('input-box2', 'value')])      
def update_output22(n_clicks,value):
    value = int(value)
    var_sig.amp = value
    return
#############################################################################################
@app.callback(
    dash.dependencies.Output('output-container3', 'children'),
    [dash.dependencies.Input('button3', 'n_clicks')],
    [dash.dependencies.State('input-box3', 'value')])      
def update_output23(n_clicks,value):
    value = int(value)
    var_sig.sam_freq = value
    return
#############################################################################################
@app.callback(
    dash.dependencies.Output('output-container4', 'children'),
    [dash.dependencies.Input('signal-type', 'value')])      
def update_output24(selected_dropdown_value):
    selected_dropdown_value = str(selected_dropdown_value)
    var_sig.sig_type = selected_dropdown_value
    return
#############################################################################################
@app.callback(
    dash.dependencies.Output('my-graph', 'figure'),
    [dash.dependencies.Input('button', 'n_clicks')])      
def update_graph(n_clicks):
    if var_sig.sig_type == 'ex':
        ts  ,exp_sig = exp_signal(var_sig.freq,var_sig.amp,var_sig.sam_freq)
        return {
        'data': [{
                    'x': ts,
                    'y': exp_sig,
                    'type': 'scatter',
                    'name': 'SF'
                }]
            }
    elif var_sig.sig_type == 'sin':
        ts  ,sin_sin = sin_signal(var_sig.freq,var_sig.amp,var_sig.sam_freq)
        return {
        'data': [{
                    'x': ts,
                    'y': sin_sin,
                    'type': 'scatter',
                    'name': 'SF'
                }]
            }
#'''CallBackTAsk3
#############################################################################################   
@app.callback(dash.dependencies.Output('output-container32', 'children'), [dash.dependencies.Input('Duraion of signal', 'value')])
def update_content31(value):
    if value == '1':
        dig.duraion=3
        return [html.Div('3 SECOND')]
    elif value == '2':
        dig.duraion=5
        return [html.Div('5 SECOND')]
    elif value == '3':
        dig.duraion=10
        return [html.Div('10 SECOND')]
    elif value == '4':
        dig.duraion=20
        return [html.Div('20 SECOND')]


# In[7]:Button To apply            

@app.callback(dash.dependencies.Output('output-container33', 'children'), [dash.dependencies.Input('type of Freq', 'value')])
def update_content32(value):
    if value == '1':
        dig.f=50
        return [html.Div('50 Hz')]
    elif value == '2':
        dig.f=250
        return [html.Div('250 Hz')]
    elif value == '3':
        dig.f=350
        return [html.Div('350 Hz')]
    elif value == '4':
        dig.f=440
        return [html.Div('440 Hz')]




@app.callback(dash.dependencies.Output('output-container31', 'children'), [dash.dependencies.Input('type of signal', 'value')])
def update_content33(value):
    fs = 44100       # sampling rate, Hz, must be integer
    p = pyaudio.PyAudio()
    if value == '1':
        samples = (np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)).astype(np.float32).tobytes()
        stream = p.open(format=pyaudio.paFloat32,channels=1,rate=fs,output=True)
        stream.write(samples)
        stream.stop_stream()
        stream.close()
        return [html.Div('one of sinosoidal signal')]
        
    elif value == '2':        
            samples = (np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)+np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)).astype(np.float32).tobytes()
            stream = p.open(format=pyaudio.paFloat32,channels=1,rate=fs,output=True)
            stream.write(samples)
            stream.stop_stream()
            stream.close()
            return [html.Div('mixture of 2 sinosoidal signal')]
    
    elif value == '3':
            samples = (np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)+np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)+np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)).astype(np.float32).tobytes()
            stream = p.open(format=pyaudio.paFloat32,channels=1,rate=fs,output=True)
            stream.write(samples)
            stream.stop_stream()
            stream.close()
            return [html.Div('mixture of 3 sinosoidal signal')]
      
    elif value == '4':
            samples = (np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)*np.exp(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)).astype(np.float32).tobytes()
            stream = p.open(format=pyaudio.paFloat32,channels=1,rate=fs,output=True)
            stream.write(samples)
            stream.stop_stream()
            stream.close()
            return [html.Div('mixture of 1 sinosoid and exponential signal')]
    
    elif value == '5':
            samples = ((np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)+np.sin(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs))*np.exp(2*np.pi*np.arange(fs*dig.duraion)*dig.f/fs)).astype(np.float32).tobytes()
            stream = p.open(format=pyaudio.paFloat32,channels=1,rate=fs,output=True)
            stream.write(samples)
            stream.stop_stream()
            stream.close()
            return [html.Div('mixture of 2 sinosoid and exponential signal')]

if __name__ == '__main__':
    app.run_server()

