# Tech-Acadamy-Live-Project
## Project Summary
The Final 2 weeks of my time with the Tech Academy were spent participating on a live ongoing webdev project with other developers. We used Azure Devops to organize a sprint and distribute stories among a rotating group. I was tasked with working on a section of the website where blog authors were to be maintained and edited. My sprint started in backend database work creating models for Authors and building out there CRUD pages. Later I would work directly on view pages using cshtml and even builing partial views to display Author profiles. Below is some of the code I contributed to the Project.

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
The database was using a code first approach and my table was drawn up from the class using Entity Framework
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
