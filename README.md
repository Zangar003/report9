# report9
func star(w http.ResponseWriter, r *http.Request) {
	db := dbConn()
	// emp := r.URL.Query().Get("id")
	if r.Method != "POST" {
		http.ServeFile(w, r, "static/templates/uploadfile.html")
		return
	}
	star := r.FormValue("star")
	emp := r.FormValue("id")
	com := r.FormValue("comment")

	delForm, err := db.Prepare("UPDATE `upload` SET  star = ? , comment = ? WHERE id = ?")
	if err != nil {
		panic(err.Error())
	}
	delForm.Exec(star, com, emp)
	log.Println("Updated successfully", emp, star, com)

	defer db.Close()
	http.Redirect(w, r, "/", 301)
}
<form style="margin: 10px;" method="post" action="/star">
						<!-- <input style="border: 1px solid; border-radius: 3px;"type="number" name="star" placeholder="1-10"/><br> -->
						<p>raiting 1-5</p>
						<select class="form-control" id="exampleFormControlSelect1" name="star">
							<option>1</option>
							<option>2</option>
							<option >3</option>
							<option>4</option>
							<option>5</option>
						</select>
						<div class="form-group">
							<label for="exampleFormControlTextarea1">write comments</label>
							<textarea class="form-control" id="exampleFormControlTextarea1" rows="3" name="comment"></textarea>
						  </div>
						<input type="checkbox"  value="{{ .ID }}" name="id"><label for="id" checked="checked" >I agree</label><br></a>
						<button type="submit" class="btn btn-primary">Submit</button>	
												</form>
          
	  
	  
	  
          This code snippet is a Go function called star that handles an HTTP POST request. When a POST request is received, the function reads the values submitted in the form, updates a row in the upload table of a MySQL database with the submitted star rating and comment, and redirects the user back to the home page.

Here is a more detailed breakdown of the code:

func star(w http.ResponseWriter, r *http.Request) is the function signature, indicating that it takes an http.ResponseWriter and an http.Request as input parameters.
db := dbConn() is a function call that creates a connection to the MySQL database.
if r.Method != "POST" { ... } checks whether the HTTP method used to submit the form was a POST request. If not, it serves a static HTML file named uploadfile.html. The return statement ensures that the function exits at this point if a GET request is received, and the code below is not executed.
star := r.FormValue("star") reads the value of the star field submitted in the form.
emp := r.FormValue("id") reads the value of the id field submitted in the form.
com := r.FormValue("comment") reads the value of the comment field submitted in the form.
delForm, err := db.Prepare("UPDATE upload SET star = ? , comment = ? WHERE id = ?") prepares a SQL statement to update the upload table with the submitted star rating and comment for the row with the specified id.
delForm.Exec(star, com, emp) executes the prepared SQL statement with the submitted star rating, comment, and id as parameters.
log.Println("Updated successfully", emp, star, com) logs a success message with the updated values.
defer db.Close() defers the closure of the database connection until the function returns.
http.Redirect(w, r, "/", 301) redirects the user back to the home page with a status code of 301 (permanent redirect).
Finally, the code snippet also includes an HTML form that allows users to submit a star rating, comment, and id value via a POST request to the server. The form includes a dropdown menu for selecting the star rating, a textarea for entering the comment, and a checkbox for agreeing to the terms of service. When the form is submitted, the function star is called to handle the POST request.

