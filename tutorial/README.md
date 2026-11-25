Run it
python -m venv venv && source venv/bin/activate # Windows: venv\Scripts\activate
pip install -r requirements.txt
cd djangotutorial
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
Then open http://127.0.0.1:8000/polls/ and http://127.0.0.1:8000/admin/

How the web works
1. What happens when you type a URL and hit Enter?
2. What are the client and the server, and how do they talk?
3. What is HTTP? What's inside a request and a response?
4. GET vs POST — when to use each?
5. What do 200, 302, 404 and 500 mean?
6. What do HTML, CSS and JavaScript each do?
7. How does a website remember who you are? (cookies and sessions)
8. How does a request travel through Django's URLs, views, models and templates?
9. Why does the voting form have {% csrf_token %}?

 URL → page. The browser parses the URL (scheme, domain, path). DNS turns the domain into an IP address. The browser opens a TCP connection, plus a TLS handshake for HTTPS, and sends an HTTP request. The server builds a response. The browser parses the HTML, fetches the CSS, JS and images it links to, and renders the page.
 Client and server. The client (your browser) makes requests, and the server (runserver here) listens on a port and answers them. They talk over HTTP, one request then one response. The server doesn't contact the client first.
 HTTP. HTTP is a text protocol with no memory between requests (stateless). A request has a method, a path, headers (Host, Cookie, User-Agent) and an optional body. A response has a status code, headers (Content-Type, Set-Cookie, Location) and a body (for example, the HTML).
 GET vs POST. GET reads data. Its parameters go in the URL, so it can be bookmarked and repeated safely. POST sends data in the body to change something on the server. In your app, viewing a question is a GET, and voting is a POST.
 Status codes. 200 OK means success. 302 Found means "go to the URL in the Location header" (your vote view redirects to results this way). 404 Not Found means the resource doesn't exist (try /polls/999/). 500 Internal Server Error means the server's code crashed.
 HTML, CSS, JS. HTML is the structure and content. CSS is the look (your green links and background image). JavaScript is behaviour that runs in the browser, such as interactivity or loading data without a page reload.
 Cookies and sessions. The server sends Set-Cookie, and the browser sends that cookie back with every later request. Django stores session data on the server and puts only a random sessionid in the cookie. That's how you stay logged in to /admin/.
 The Django flow. Request → middleware → mysite/urls.py → include() → polls/urls.py matches the path and captures pk → the view runs → the model and ORM query the database → render() fills the template with the context → HttpResponse → middleware → browser. Trace /polls/1/ as your example.
 CSRF. Cross-Site Request Forgery: another website could make your browser POST a vote using your cookies, without you knowing. {% csrf_token %} adds a secret hidden field. Django's CSRF middleware rejects any POST without the matching token, with a 403 error. To see this, remove the tag and try to vote.ś
