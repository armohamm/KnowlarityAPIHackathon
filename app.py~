#appcfg.py -A enduring-grid-600 update appengine-try-python
from flask import Flask, render_template, request, jsonify
import datetime
import MySQLdb
app = Flask(__name__)

# main index
@app.route("/")
def index():
	return render_template("index.html")

def connect_it():
	db = MySQLdb.connect("us-cdbr-iron-east-01.cleardb.net","b5d17a9a9a98e9","3675c689","heroku_a657c2307ce439d" )
	return db

@app.route("/IVR", methods=["GET","POST"])
def api_call():
	passed_no = request.args.get('mobile_no')
	ngo_id = request.args.get('ngo_id')
	db = connect_it()
	cursor = db.cursor()
	date_time = datetime.datetime.strftime(datetime.datetime.now(),'%d-%m-%Y %H:%M %p')
	sql = "INSERT INTO contact(ngo_id, mobile, query_date) VALUES ('%s', '%s','%s' )" % (ngo_id, passed_no, date_time)
	try:
   		# Execute the SQL command
   		cursor.execute(sql)
   		# Fetch all the rows in a list of lists.
		db.commit()
   		"""results = cursor.fetchall()
   		for row in results:
	        	mobile = row[0]
      			query_date= row[1]"""
      # Now print fetched result
	except:
   		db.rollback()
# disconnect from server
	db.close()
	
	return "Successfully inserted into db: "+str(ngo_id)+" "+str(passed_no)+" "+str(date_time)

@app.route("/donationQuery")
def donationQuery():
    db = connect_it()
    cursor = db.cursor()
    sql = "select * from contact"
    cursor.execute(sql)
    results = cursor.fetchall()
    details = []
    for row in results:
    	ngo_id = row[0]
    	mobile_no= row[1]
	query_time = row[2]
        url = "http://dev.knowlarity.com/api/voice/quickCall/?username=sumit786raj@gmail.com&password=000786&ivr_id=800067070&phone_book='"+ mobile_no +"'&format=xml"
    	details.append({'ngo_id':ngo_id,'mobile_no':mobile_no,'query_time':query_time,'url':url})
        
    details = sorted(details, key=lambda x:x['ngo_id'])
    
    #TODO get the details from db and pass to donation details.html
    return render_template("details.html", details=details)

if __name__ == "__main__":
	app.run(debug=True)
