import React, { useState, useRef, useEffect } from 'react';
import { MessageSquare, Calendar, BarChart2, User, Send, Check, X, Edit, Clock } from 'lucide-react';

const MarkoAI = () => {
  const [activeTab, setActiveTab] = useState('chat');
  const [messages, setMessages] = useState([
    { 
      id: 1, 
      sender: 'marko', 
      text: 'Good morning, Dominick! Your social posts from last week saw a 22% increase in engagement. Would you like me to create some new content for this week?',
      time: '9:02 AM'
    }
  ]);
  const [inputMessage, setInputMessage] = useState('');
  const [generatedContent, setGeneratedContent] = useState(null);
  const [isMarkoTyping, setIsMarkoTyping] = useState(false);
  
  const messagesEndRef = useRef(null);
  
  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  };
  
  useEffect(() => {
    scrollToBottom();
  }, [messages]);
  
  const handleSendMessage = () => {
    if (inputMessage.trim() === '') return;
    
    // Add user message
    const newUserMessage = {
      id: messages.length + 1,
      sender: 'user',
      text: inputMessage,
      time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
    };
    
    setMessages([...messages, newUserMessage]);
    setInputMessage('');
    setIsMarkoTyping(true);
    
    // Simulate Marko's response after a delay
    setTimeout(() => {
      if (inputMessage.toLowerCase().includes('facebook') || inputMessage.toLowerCase().includes('social')) {
        // Show content generation response
        const markoResponse = {
          id: messages.length + 2,
          sender: 'marko',
          text: "I've created some Facebook post options based on your business goals and brand voice. You can edit them if needed or approve to schedule.",
          time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };
        setMessages(prevMessages => [...prevMessages, markoResponse]);
        
        // Set generated content
        setGeneratedContent({
          platform: 'Facebook',
          options: [
            "Spring has sprung at Morgan's Renovations! 🏡 Ready to refresh your home? Our April special offers 15% off kitchen upgrades. Book your free consultation today! #HomeRenovation #SpringRefresh",
            "Transform your space without breaking the bank! Check out our latest blog post on 'Budget-Friendly Home Upgrades That Make a Big Impact.' Link in bio. #HomeImprovement #BudgetRenovation"
          ]
        });
      } else {
        // Regular response
        const markoResponse = {
          id: messages.length + 2,
          sender: 'marko',
          text: "I'm here to help with your marketing needs. Would you like me to create content, check your analytics, or help with marketing strategy?",
          time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };
        setMessages(prevMessages => [...prevMessages, markoResponse]);
      }
      setIsMarkoTyping(false);
    }, 1500);
  };
  
  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleSendMessage();
    }
  };
  
  const handleApproveContent = (index) => {
    const selectedContent = generatedContent.options[index];
    const confirmationMessage = {
      id: messages.length + 1,
      sender: 'marko',
      text: `Great! I've scheduled "${selectedContent.substring(0, 40)}..." to post tomorrow at 10:00 AM. Would you like me to create any other content?`,
      time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
    };
    setMessages([...messages, confirmationMessage]);
    setGeneratedContent(null);
  };
  
  const handleQuickAction = (action) => {
    const quickMessage = {
      id: messages.length + 1,
      sender: 'user',
      text: action,
      time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
    };
    setMessages([...messages, quickMessage]);
    setIsMarkoTyping(true);
    
    setTimeout(() => {
      let response;
      if (action === "Create social post") {
        response = {
          id: messages.length + 2,
          sender: 'marko',
          text: "What platform would you like to create content for? I can help with Facebook, Instagram, Twitter, LinkedIn, or all of them.",
          time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };
      } else if (action === "Check performance") {
        response = {
          id: messages.length + 2,
          sender: 'marko',
          text: "Your marketing is performing well this month! You've seen a 22% increase in social engagement and your email open rate is at 28%, which is above industry average. Your website traffic has increased by 15% compared to last month.",
          time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };
      } else {
        response = {
          id: messages.length + 2,
          sender: 'marko',
          text: "What type of marketing strategy would you like help with? I can suggest content ideas, audience targeting, or campaign optimization.",
          time: new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };
      }
      setMessages(prevMessages => [...prevMessages, response]);
      setIsMarkoTyping(false);
    }, 1500);
  };
  
  // Dashboard data
  const dashboardData = {
    socialEngagement: {
      current: 22,
      trend: 'up',
      metric: '% increase'
    },
    websiteTraffic: {
      current: 1250,
      trend: 'up',
      metric: 'visitors'
    },
    emailPerformance: {
      current: 28,
      trend: 'up',
      metric: '% open rate'
    },
    upcomingPosts: 3
  };
  
  // Calendar data
  const calendarData = [
    { id: 1, title: "Spring renovations promo", platform: "Facebook", date: "Mar 21", time: "10:00 AM", status: "scheduled" },
    { id: 2, title: "Home office design tips", platform: "Instagram", date: "Mar 22", time: "2:00 PM", status: "scheduled" },
    { id: 3, title: "Monthly newsletter", platform: "Email", date: "Mar 25", time: "9:00 AM", status: "scheduled" }
  ];
  
  return (
    <div className="flex flex-col h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-white shadow-sm py-4 px-6">
        <div className="flex justify-between items-center">
          <div className="flex items-center space-x-2">
            <div className="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center">
              <span className="text-white font-bold">M</span>
            </div>
            <h1 className="text-xl font-bold text-blue-800">Marko.ai</h1>
          </div>
          <div className="flex items-center space-x-2">
            <span className="text-sm text-gray-600">Dominick Morgan</span>
            <div className="w-8 h-8 bg-gray-200 rounded-full"></div>
          </div>
        </div>
      </header>
      
      {/* Main content */}
      <div className="flex flex-1 overflow-hidden">
        {/* Navigation */}
        <nav className="w-16 bg-white border-r border-gray-200 flex flex-col items-center py-4">
          <button 
            className={`p-3 rounded-xl mb-4 ${activeTab === 'chat' ? 'bg-blue-100 text-blue-600' : 'text-gray-500 hover:bg-gray-100'}`}
            onClick={() => setActiveTab('chat')}
          >
            <MessageSquare size={20} />
          </button>
          <button 
            className={`p-3 rounded-xl mb-4 ${activeTab === 'calendar' ? 'bg-blue-100 text-blue-600' : 'text-gray-500 hover:bg-gray-100'}`}
            onClick={() => setActiveTab('calendar')}
          >
            <Calendar size={20} />
          </button>
          <button 
            className={`p-3 rounded-xl mb-4 ${activeTab === 'analytics' ? 'bg-blue-100 text-blue-600' : 'text-gray-500 hover:bg-gray-100'}`}
            onClick={() => setActiveTab('analytics')}
          >
            <BarChart2 size={20} />
          </button>
          <button 
            className={`p-3 rounded-xl ${activeTab === 'profile' ? 'bg-blue-100 text-blue-600' : 'text-gray-500 hover:bg-gray-100'}`}
            onClick={() => setActiveTab('profile')}
          >
            <User size={20} />
          </button>
        </nav>
        
        {/* Content area */}
        <div className="flex-1 overflow-hidden">
          {activeTab === 'chat' && (
            <div className="flex flex-col h-full">
              {/* Chat messages */}
              <div className="flex-1 p-4 overflow-y-auto bg-gray-50">
                {messages.map(message => (
                  <div 
                    key={message.id} 
                    className={`flex ${message.sender === 'user' ? 'justify-end' : 'justify-start'} mb-4`}
                  >
                    {message.sender === 'marko' && (
                      <div className="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center mr-2">
                        <span className="text-white font-bold">M</span>
                      </div>
                    )}
                    <div>
                      <div 
                        className={`max-w-md rounded-lg py-2 px-4 ${
                          message.sender === 'user' 
                            ? 'bg-blue-600 text-white rounded-br-none' 
                            : 'bg-white text-gray-800 shadow-sm rounded-bl-none'
                        }`}
                      >
                        <p>{message.text}</p>
                      </div>
                      <p className="text-xs text-gray-500 mt-1">{message.time}</p>
                    </div>
                    {message.sender === 'user' && (
                      <div className="w-8 h-8 bg-gray-200 rounded-full ml-2"></div>
                    )}
                  </div>
                ))}
                
                {isMarkoTyping && (
                  <div className="flex justify-start mb-4">
                    <div className="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center mr-2">
                      <span className="text-white font-bold">M</span>
                    </div>
                    <div className="bg-white text-gray-800 rounded-lg py-2 px-4 shadow-sm rounded-bl-none">
                      <div className="flex space-x-1">
                        <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
                        <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{animationDelay: '0.2s'}}></div>
                        <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{animationDelay: '0.4s'}}></div>
                      </div>
                    </div>
                  </div>
                )}
                
                {generatedContent && (
                  <div className="bg-white rounded-lg shadow-sm p-4 mb-4 border-l-4 border-blue-600">
                    <h3 className="font-medium text-gray-800 mb-2">Generated {generatedContent.platform} Content</h3>
                    {generatedContent.options.map((option, index) => (
                      <div key={index} className="bg-gray-50 p-3 rounded mb-3 relative">
                        <p className="pr-24">{option}</p>
                        <div className="absolute right-2 top-2 flex space-x-1">
                          <button 
                            className="p-1 rounded hover:bg-green-100"
                            onClick={() => handleApproveContent(index)}
                          >
                            <Check size={16} className="text-green-600" />
                          </button>
                          <button className="p-1 rounded hover:bg-blue-100">
                            <Edit size={16} className="text-blue-600" />
                          </button>
                          <button className="p-1 rounded hover:bg-red-100">
                            <X size={16} className="text-red-600" />
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                )}
                
                <div ref={messagesEndRef} />
              </div>
              
              {/* Quick actions */}
              <div className="bg-white border-t border-gray-200 p-2">
                <div className="flex space-x-2 mb-2 overflow-x-auto pb-2">
                  <button 
                    className="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-sm whitespace-nowrap"
                    onClick={() => handleQuickAction("Create social post")}
                  >
                    Create social post
                  </button>
                  <button 
                    className="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-sm whitespace-nowrap"
                    onClick={() => handleQuickAction("Check performance")}
                  >
                    Check performance
                  </button>
                  <button 
                    className="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-sm whitespace-nowrap"
                    onClick={() => handleQuickAction("Marketing strategy")}
                  >
                    Marketing strategy
                  </button>
                </div>
                
                {/* Message input */}
                <div className="flex items-center bg-gray-100 rounded-lg px-3 py-2">
                  <input 
                    type="text" 
                    className="flex-1 bg-transparent outline-none"
                    placeholder="Ask Marko anything about your marketing..."
                    value={inputMessage}
                    onChange={(e) => setInputMessage(e.target.value)}
                    onKeyPress={handleKeyPress}
                  />
                  <button 
                    className="ml-2 p-1 rounded-full bg-blue-600 text-white"
                    onClick={handleSendMessage}
                  >
                    <Send size={18} />
                  </button>
                </div>
              </div>
            </div>
          )}
          
          {activeTab === 'calendar' && (
            <div className="p-6 h-full overflow-y-auto">
              <h2 className="text-xl font-bold mb-4">Content Calendar</h2>
              
              <div className="grid grid-cols-7 gap-2 mb-4">
                {['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'].map(day => (
                  <div key={day} className="text-center font-medium text-gray-500">{day}</div>
                ))}
                
                {Array.from({ length: 35 }, (_, i) => (
                  <div 
                    key={i} 
                    className={`aspect-square bg-white border rounded-lg flex flex-col items-center justify-start p-1 ${
                      i === 20 || i === 21 || i === 24 ? 'border-blue-200 bg-blue-50' : ''
                    }`}
                  >
                    <span className="text-xs font-medium">{i + 1}</span>
                    {i === 20 && <div className="w-4 h-1 bg-blue-500 rounded-full mt-1"></div>}
                    {i === 21 && <div className="w-4 h-1 bg-pink-500 rounded-full mt-1"></div>}
                    {i === 24 && <div className="w-4 h-1 bg-green-500 rounded-full mt-1"></div>}
                  </div>
                ))}
              </div>
              
              <h3 className="font-medium text-gray-800 mb-2">Upcoming Content</h3>
              <div className="bg-white rounded-lg shadow-sm divide-y divide-gray-100">
                {calendarData.map(item => (
                  <div key={item.id} className="p-4 flex items-center">
                    <div className="mr-4">
                      <div className="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center">
                        {item.platform === 'Facebook' && <span className="text-blue-600 font-bold">F</span>}
                        {item.platform === 'Instagram' && <span className="text-pink-600 font-bold">I</span>}
                        {item.platform === 'Email' && <span className="text-green-600 font-bold">E</span>}
                      </div>
                    </div>
                    <div className="flex-1">
                      <h4 className="font-medium">{item.title}</h4>
                      <p className="text-sm text-gray-500">{item.platform} • {item.date} at {item.time}</p>
                    </div>
                    <div className="flex items-center text-sm text-gray-500">
                      <Clock size={14} className="mr-1" />
                      <span>{item.status}</span>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}
          
          {activeTab === 'analytics' && (
            <div className="p-6 h-full overflow-y-auto">
              <h2 className="text-xl font-bold mb-4">Marketing Analytics</h2>
              
              <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
                <div className="bg-white p-4 rounded-lg shadow-sm">
                  <h3 className="text-sm text-gray-500 mb-1">Social Engagement</h3>
                  <div className="flex items-baseline">
                    <span className="text-2xl font-bold mr-2">{dashboardData.socialEngagement.current}%</span>
                    <span className="text-sm text-green-600">↑ from last month</span>
                  </div>
                  <div className="mt-4 h-16 flex items-end space-x-1">
                    {[30, 45, 25, 50, 35, 40, 65, 55, 50, 75, 60, 85].map((h, i) => (
                      <div 
                        key={i} 
                        className="flex-1 bg-blue-500 rounded-t"
                        style={{ height: `${h}%` }}
                      ></div>
                    ))}
                  </div>
                </div>
                
                <div className="bg-white p-4 rounded-lg shadow-sm">
                  <h3 className="text-sm text-gray-500 mb-1">Website Traffic</h3>
                  <div className="flex items-baseline">
                    <span className="text-2xl font-bold mr-2">{dashboardData.websiteTraffic.current}</span>
                    <span className="text-sm text-green-600">↑ 15% from last month</span>
                  </div>
                  <div className="mt-4 h-16 flex items-end space-x-1">
                    {[40, 35, 45, 30, 55, 50, 45, 60, 50, 70, 65, 75].map((h, i) => (
                      <div 
                        key={i} 
                        className="flex-1 bg-green-500 rounded-t"
                        style={{ height: `${h}%` }}
                      ></div>
                    ))}
                  </div>
                </div>
              </div>
              
              <h3 className="font-medium text-gray-800 mb-2">Performance Insights</h3>
              <div className="bg-white rounded-lg shadow-sm p-4 mb-4">
                <div className="flex items-start">
                  <div className="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center mr-3 mt-1">
                    <span className="text-white font-bold">M</span>
                  </div>
                  <div>
                    <p className="mb-2">Your posts featuring customer testimonials are getting 40% more engagement than other content. Consider creating more testimonial posts this month.</p>
                    <button className="text-sm text-blue-600 font-medium">Create testimonial post</button>
                  </div>
                </div>
              </div>
              
              <div className="bg-white rounded-lg shadow-sm p-4">
                <div className="flex items-start">
                  <div className="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center mr-3 mt-1">
                    <span className="text-white font-bold">M</span>
                  </div>
                  <div>
                    <p className="mb-2">Your email open rates are 28%, which is above the industry average of 22%. The most engaging subject lines use questions and mention specific services.</p>
                    <button className="text-sm text-blue-600 font-medium">Plan next email campaign</button>
                  </div>
                </div>
              </div>
            </div>
          )}
          
          {activeTab === 'profile' && (
            <div className="p-6 h-full overflow-y-auto">
              <h2 className="text-xl font-bold mb-4">Brand Profile</h2>
              
              <div className="bg-white rounded-lg shadow-sm p-6 mb-6">
                <div className="flex items-center mb-6">
                  <div className="w-16 h-16 bg-gray-200 rounded-full mr-4"></div>
                  <div>
                    <h3 className="text-lg font-bold">Morgan's Renovations</h3>
                    <p className="text-gray-600">Home renovation services</p>
                  </div>
                </div>
                
                <h4 className="font-medium text-gray-800 mb-2">About</h4>
                <p className="text-gray-600 mb-4">
                  Morgan's Renovations provides high-quality home renovation services with a focus on kitchens, 
                  bathrooms, and home offices. We pride ourselves on attention to detail and customer satisfaction.
                </p>
                
                <h4 className="font-medium text-gray-800 mb-2">Brand Voice</h4>
                <div className="flex space-x-2 mb-4">
                  <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm">Professional</span>
                  <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm">Friendly</span>
                  <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm">Helpful</span>
                </div>
                
                <h4 className="font-medium text-gray-800 mb-2">Target Audience</h4>
                <p className="text-gray-600">
                  Homeowners aged 30-65 in the Chicago area with household incomes above $75,000 who are 
                  looking to improve their living spaces for comfort and value.
                </p>
              </div>
              
              <h3 className="font-medium text-gray-800 mb-2">Connected Platforms</h3>
              <div className="bg-white rounded-lg shadow-sm divide-y divide-gray-100">
                <div className="p-4 flex items-center">
                  <div className="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center mr-4">
                    <span className="text-blue-600 font-bold">F</span>
                  </div>
                  <div className="flex-1">
                    <h4 className="font-medium">Facebook</h4>
                    <p className="text-sm text-gray-500">Connected</p>
                  </div>
                  <button className="text-sm text-blue-600 font-medium">Edit</button>
                </div>
                
                <div className="p-4 flex items-center">
                  <div className="w-10 h-10 rounded-full bg-pink-100 flex items-center justify-center mr-4">
                    <span className="text-pink-600 font-bold">I</span>
                  </div>
                  <div className="flex-1">
                    <h4 className="font-medium">Instagram</h4>
                    <p className="text-sm text-gray-500">Connected</p>
                  </div>
                  <button className="text-sm text-blue-600 font-medium">Edit</button>
                </div>
                
                <div className="p-4 flex items-center">
                  <div className="w-10 h-10 rounded-full bg-green-100 flex items-center justify-center mr-4">
                    <span className="text-green-600 font-bold">E</span>
                  </div>
                  <div className="flex-1">
                    <h4 className="font-medium">Email Marketing</h4>
                    <p className="text-sm text-gray-500">Connected</p>
                  </div>
                  <button className="text-sm text-blue-600 font-medium">Edit</button>
                </div>
              </div>
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

export default MarkoAI;
