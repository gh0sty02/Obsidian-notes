![[Pasted image 20260319174109.png]]

# Requirements

The goal is to clear requirements of the system

1. Functional Requirements
   - These are "User/Clients Should be able to..." questions
   - ask targeted questions as if you are talking to a customer, client or product manager
   - ask questions such as - does the system need to do X?
   - there might be hundreds of features but you have to prioritize on the top 3
   - eg : if you were designing a system like twitter, you might have the following FR: 
     - Users should be able to post tweets
     - users should be able to follow other users
     - users should be able to see tweets from the users they follow

2. Non Functional Requirements
   - These are statements about the system qualities that are important for your users
   - can be phrased as "The system should be able to " or "The system should be.."
   - eg: in the example of twitter, NFR would be: 
	   - the system should be highly scalable, prioritize availability over consistency
	   - should be able to scale to suport 100M+ DAU
	   - should be low latency, rendering tools under 200ms
	   - should be quantifiable- eg : the system should have low latency search < 500ms   
   
   - Here is the checklist to identify the top 3-5 NFRs 
	- CAP Theorem 
		- should the system prioritize consistency or availability? 
	- Environment Constrains
		- are there any constraints on the environment in which the system will run. 
		- eg : Are you running the system on mobile device with limited battery, running on devices with low memory or bandwidth
	- Scalability:
		- Does the system have unique scaling requirement ? 
		- eg : 
			- does it have a busty traffic at a specific time of the day
			- are there any events, holidays which will cause a significant increase in traffic
			- does the system needs to scale reads or write
	- Latency: 
		- how quickly does the system need to respond to user request ? specifically for requests that require meaningful computation. eg : search latency
	- Durability:
		- how important is it that the data in your system is not lost. eg : social network might be okay with some data loss but not banking systems
	- Security: 
		- how secure does the system need to be ? consider data protection, access control, etc
	- Fault Tolerance : 
		- how well the system need to handle failures ? consider redunduncy and recovery mechanisms
	- Compliance: 
		- are there any legal or regulatory requirements the system needs to meet ? 

3. Capacity Estimation
	- not required in many cases but maybe required in some cases

4. Core entities:
	- entities that your api will exachange
	- model that will be used to persist data
	- start with small list and then build out as you go
	- questions to ask to help identify core entities:
		- who are the actors in the system ? are they overlapping
		- what are the nouns or resources necessary to satisfy the functional requirements

5. API or System Interface
	- use REST for default api and design endpoints.
	- eg : 
		```
		POST /v1/tweets
		body : {
			"text": string
		}
		
		POST v1/tweets/{tweetId} 
		```
5. Data Flow (Optional)
	- might be useful for data-processing systems where we have to define a set of actions or processes that the system performs to produce the desired output

