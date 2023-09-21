---
layout: post
title:  "React with REST APIs (1)"
date:   2023-09-18 00:00:00 -0000
categories: [react, api, tutorial]
---
# React with mock APIs:

    https://64e7bf5db0fd9648b7904d83.mockapi.io/fakeData

# Test: 

    https://demo-react-nine-sigma.vercel.app/
    
    https://demo-react-weld.vercel.app/

* Step 1: Install Create React App : npm install -g create-react-app
* Step 2: Create a New React Project: npx create-react-app demo-react
* Step 3: cd demo-react
* Step 4: modify \src\app.js  

        import './App.css'; 
        import Create from './components/create';
        import Read from './components/read';
        import Update from './components/update'; 
        import { BrowserRouter as Router, Routes,  Route} from 'react-router-dom'
        

        function App() { 
        return ( 
            <>  
            <Router>
                <div className="main">
                    <h2 className="main-header">React Crud Operations</h2>
                    <Routes> 
                    <Route index element={<Read />} />
                    <Route path='/create' element={<Create />} />
                    <Route path='/read' element={<Read />} /> 
                    <Route path="/update/:_id" element={<Update />} /> 
                    </Routes>
                </div>
            </Router>
            </>
            ) 
        }   

        export default App;

* Step 5: add folder components, and add three files inside: create.js, read.
js and update.js  

# The complete code can be referred at : 

    https://github.com/houzhihuil/demo-react
