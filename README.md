# Tech-Acadamy-Live-Project
## Project Summary
The Final 2 weeks of my time with the Tech Academy were spent participating on a live ongoing webdev project with other developers. We used Azure Devops to organize a sprint and distribute stories among a rotating group. I was tasked with working on a section of the website where blog authors were to be maintained and edited. My sprint started in backend database work creating models for Authors and building out there CRUD pages. Later I would work directly on view pages using cshtml and even builing partial views to display Author profiles. Below is some of the code I contributed to the Project.

* [Model](#database-model)
* [Views](#views)
* [Partial View](#partial-view)
* [Helper Tool](#helper-text-tool)
* [More Lessons](#more-lessons)

## Examples

### Database model

A BlogAuthor model was going to be used as the base for work during the 2 week sprint. An ID, strings, and dates with some specific formatting were created.
```
public class BlogAuthor
    {
        public int BlogAuthorId { get; set; }
        public string Name { get; set; }
        public string Bio { get; set; }
        [DisplayFormat(ApplyFormatInEditMode = true, DataFormatString = "{0:yyyy-MM-ddThh:mm}")]
        public DateTime Joined { get; set; }
        [DisplayFormat(ApplyFormatInEditMode = true, DataFormatString = "{0:yyyy-MM-ddThh:mm}")]
        public DateTime? Left { get; set; }
    }
```
The database was using a code first approach and my table was drawn up from the class using Entity Framework.
```
CREATE TABLE [dbo].[BlogAuthors] (
    [BlogAuthorId] INT            IDENTITY (1, 1) NOT NULL,
    [Name]         NVARCHAR (MAX) NULL,
    [Bio]          NVARCHAR (MAX) NULL,
    [Joined]       DATETIME       NOT NULL,
    [Left]         DATETIME       NULL,
    CONSTRAINT [PK_dbo.BlogAuthors] PRIMARY KEY CLUSTERED ([BlogAuthorId] ASC)
);
```
### Views

I worked a lot on the different views for the blog author pages, changing the layouts and adding different functions. The first one I spent time on was a create page.
Bootstrap was used to design the front end layouts and would later play a bigger role in some of the functions.
```
<div class="BlogAuthorForm">
        <h4>BlogAuthor</h4>
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        <div class="FormBackground">
            <div class="form-group">
                @Html.LabelFor(model => model.Name, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col">
                    @Html.EditorFor(model => model.Name, new { htmlAttributes = new { @class = "form-control" } })
                    @Html.ValidationMessageFor(model => model.Name, "", new { @class = "text-danger" })
                </div>
            </div>

            <div class="form-group">
                @Html.LabelFor(model => model.Bio, htmlAttributes: new { @class = "control-label col" })
                <div class="col">
                    @Html.TextAreaFor(model => model.Bio, new  { @class = "form-control", rows="10" } )
                    @Html.ValidationMessageFor(model => model.Bio, "", new { @class = "text-danger" })
                </div>
            </div>
            
            <div class="row justify-content-center">
                <div class="form-group">
                    @Html.LabelFor(model => model.Joined, htmlAttributes: new { @class = "control-label col" })
                    <div class="col">
                        @Html.EditorFor(model => model.Joined, new { htmlAttributes = new { @type = "datetime-local", @class = "form-control" } })
                        @Html.ValidationMessageFor(model => model.Joined, "", new { @class = "text-danger" })
                    </div>
                </div>

                <div class="form-group">
                    @Html.LabelFor(model => model.Left, htmlAttributes: new { @class = "control-label col" })
                    <div class="col">
                        @Html.EditorFor(model => model.Left, new { htmlAttributes = new { @type = "datetime-local", @class = "form-control" } })
                        @Html.ValidationMessageFor(model => model.Left, "", new { @class = "text-danger" })
                    </div>
                </div>
            </div>

            <div class="form-group">
                <div class="col-md-offset-2 col-md-10">
                    <a class="btn btn-secondary" href="/Blog/BlogAuthors/Index">Back to List</a>
                    <input type="submit" value="Create" class="btn btn-success" />
                </div>
            </div>
        </div>
    </div>
```

### Partial view

Creating a partial view of the Author details that could be stacked on a page with individual functions became a multi staged story that took a few forms.
The initial solution I had created used Javascript to control buttons on each partial. As a single view this method worked great but I had trouble seperating each partial views element IDs and caused some overlapping functions. To Fix this I utilized cshtml and bootstrap capabilities to have unique IDs that would let each partial function amoung many on one view.

```
@model TheatreCMS3.Areas.Blog.Models.BlogAuthor

@{ 
    string authorTabId = $"author-tab-{Model.BlogAuthorId}";
    string blogTabId = $"blog-tab-{Model.BlogAuthorId}";
    string contactTabId = $"contact-tab-{Model.BlogAuthorId}";
    string twitterTabId = $"twitter-tab-{Model.BlogAuthorId}";

    string authorContentId = $"author-content-{Model.BlogAuthorId}";
    string blogContentId = $"blog-content-{Model.BlogAuthorId}";
    string contactContentId = $"contact-content-{Model.BlogAuthorId}";
    string twitterContentId = $"twitter-content-{Model.BlogAuthorId}"; 
}

<div class="AuthorBtnContainer">
    <div class="AuthorBtnPlacement pt-1 justify-content-center">
        <ul class="nav nav-pills justify-content-center" role="tablist">
            <li class="nav-item" role="presentation">
                <a class="nav-link active" id="@authorTabId" href="@($"#{authorContentId}")" data-toggle="pill" role="tab" aria-controls="@authorContentId" aria-selected="true">Author Details</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" id="@blogTabId" href="@($"#{blogContentId}")" data-toggle="pill" role="tab" aria-controls="@blogContentId" aria-selected="false">Blog Post</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" id="@contactTabId" href="@($"#{contactContentId}")" data-toggle="pill" role="tab" aria-controls="@contactContentId" aria-selected="false">Contact</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" id="@twitterTabId" href="@($"#{twitterContentId}")" data-toggle="pill" role="tab" aria-controls="@twitterContentId" aria-selected="false">Twitter</a>
            </li>
        </ul>
    </div>
</div>
<!----------------------top buttons------------------->
<div class="border-0 card text-center">
    <div>
        <div class="row m-3">
            <!---------------------Portrait--------------------->
            <div class="col-lg-4">
                <img src="https://via.placeholder.com/250x300.jpeg?text=Author+Portrait" alt="Author Portrait">
            </div>
            <!---------------------End Portrait--------------------->
            <div class="col-lg-8">
                <div class="tab-content" id="@($"tabContent-{Model.BlogAuthorId}-pills")">
                    <div class="AuthorDetails tab-pane active show" id="@authorContentId" role="tabpanel" aria-labelledby="@authorTabId">
                        <h5 class="card-title">
                            @Html.DisplayFor(model => model.Name)

                            <!---------------------------social media icons------------------------------->
                            <span style="float:right">
                                <a href="https://www.facebook.com/"><i class="fab fa-facebook"></i></a>&nbsp;
                                <a href="https://www.instagram.com/"><i class="fab fa-instagram"></i></a>
                            </span>
                            <!--------------------------end social media icons------------------------------>
                        </h5>

                        <p class="card-text">@Html.DisplayFor(model => model.Bio)</p>
                        <div class="row">
                            <div class="col">
                                <p><strong>Joined Date</strong></p>
                                <p>@Html.DisplayFor(model => model.Joined)</p>
                            </div>
                            <div class="col">
                                <p><strong>Left Date</strong></p>
                                <p>@Html.DisplayFor(model => model.Left)</p>
                            </div>
                        </div>
                    </div>
                    <div class="AuthorDetails tab-pane" id="@blogContentId" role="tabpanel" aria-labelledby="@blogTabId">
                        <h5 class="card-title">Post</h5>
                        <p class="card-text">
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                            Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum
                        </p>
                    </div>
                    <div class="AuthorDetails tab-pane" id="@contactContentId" role="tabpanel" aria-labelledby="@contactTabId">
                        (Implement in future stories)
                    </div>
                    <div class="AuthorDetails tab-pane" id="@twitterContentId" role="tabpanel" aria-labelledby="@twitterTabId">
                        (Implement in future stories)
                    </div>
                </div>
                
            </div>
        </div>
    </div>
    <!-----------------Edit button------------------->
    <div>
        <a href="@Url.Action("Delete", "BlogAuthors", new { id = Model.BlogAuthorId })" class="btn BtnAuthorDelete">
            <i class="fas fa-trash"></i>
        </a>
        <a href="@Url.Action("Edit", "BlogAuthors", new { id = Model.BlogAuthorId })" class="btn BtnAuthorEdit">
            <i class="fas fa-edit"></i>
            <span>
                <strong>Edit</strong>
            </span>
        </a>
    </div>
    <!-----------------End Edit button------------------->

</div>
```

This is the index page that utilized each of my partial views.
```
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.BlogAuthor>
@using TheatreCMS3.Areas.Blog.Models;

@{
    ViewBag.Title = "Index";
}

<h2>Index</h2>

<p>
    @Html.ActionLink("Create New", "Create")
</p>
@foreach (BlogAuthor item in Model) {
    @Html.Partial("_BlogAuthor", (BlogAuthor) item);
    <br />
}
```

### Helper text tool
A smaller story I worked on was a text tool that limited characters in a block of text and added an ellipses.

```
public class TextHelpers
    {
        public static string LimitCharacters(string targetText, int limit)
        {
            if (targetText.Length > limit)
            {
                // checks if string surpasses limit
                targetText = targetText.Substring(0, limit);

                // checks if the string ends in a space. If so it is removed.
                string spaceCheck = targetText.Substring(targetText.Length - 1);
                if (spaceCheck == " ")
                {
                    targetText = targetText.Remove(targetText.Length - 1, 1);
                }
                // concatinates an ellipses to the string
                targetText = targetText + "...";
            }

            return targetText;
        }
    }
```
[back to top](#project-summary)

## More Lessons

Along side learning a lot with coding itself I gained a lot of invaluable knowlege about version control, agile/scrum, and working on existing code.
Utilizing branches to try out ideas and keeping my code away from the master branch until it was ready gave me a sense of safty to make mistakes while looking for the best solution. Joining standups everyday with the team and descussing our daily task and recent successes/struggles helped bring the team closer and made our individual assignments feel more meaningful. Jumping into a already running web application made it pretty easy to begin work and contribute immediatly. Being able to reference style guides and layout let me worry about one less thing and focus on just writing code.
