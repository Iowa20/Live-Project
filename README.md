# Live-Project
For my final project at The Tech Academy, I worked with a development team on a full-scale MVC/MVVM in C#. It was a great opportunity to take on a legacy code base, see how the code was laid out, fix bugs, and add requested features. I saw first-hand how an app can evolve over time and outgrow some of the early choices the original developers made while building it. When changing direction on big decisions would require a larger rewrite that the client does not have time for, I saw how good developers can pick up what was there and make the best of the situation to deliver a quality product. We worked together as a team to learn the quirks of how the application was first written and how we could work within those constraints to still deliver the desired what was asked of us. During the two-week project, I worked on several back end stories that I am very proud of. Because much of the site had already been built, there were also a good deal of front end stories and UX improvements that needed to be completed, all of varying degrees of difficulty. We shared the stories available so that everyone on the team would have a chance to work on some front end and some back end content. Over the two week sprint I also had the opportunity to work on some other project management and team programming skills that I'm confident I will use again and again on future projects.

Here are th codes for the Calendar Index that I've ressolved.
I used develpore tools to navigate the Divs and the spaces to fix the calendar to a center position.
@model ManagementPortal.Models.CalendarEvent
@{
    ViewBag.Title = "Index";
}


@*Notes and comments for the developers that come after...

--To render this view spin up the project and click on the collapsed time off calendar link within the home page nav bar.  You should see the calender being rendered currently.--


This view page currently is utilized as a hub from which to render a custom version of the FullCalendar suggested in the original story.
Full Calendar has been a project for maybe over 10 years... feel free to research.  Its a great foundation from which you can leverage and customize a really neat calender.

Bringing it into a current version using MVC and Code First and successfully implementing within a large complicated project like this one was challenging.

As it stands you have a working database, a controller, two models, and a current view page.  Feel free to lookover all of them.

This page includes all of the custom html and js that is used to add functionality to your calendar.  You may ultimately end up moving them to the site js page, maybe not.

So you can see what the event forms look like so you understand if you still have rendering issues or not.  I recommend reviewing this tutorial which was a big help to getting started on
adding CRUD to the calendar.  http://www.dotnetawesome.com/2017/06/event-calendar-in-aspnet-mvc.html  (huge help... check out the forms in part 2 especially how datetimepicker is supposed to render.)

Right now you can click and drag your mouse over 1 day or several to start to create an event.  You can do so in any view.  You can select and edit. Drag and drop... It's very versatile.

The calendar is Functional... The issue after adding it to the project is that the datetimepicker that allows for specific date and/or time selection is not rendering due to some invisible blockage 
from somewhere.  I ran out of time before I could locate the issue specifically.

I suspect this will all be resolved through front end work customizing the styles for this particular view.

So far attempts to leverage using the BundleConfigs has not worked together with the Shared/_Layout page...

I used the browser inspect tools to play with some alternative css solutions and have a added a couple to the site.css page in order to help you see the calendar as it was transparent after 
porting my code into the project. 

Don't fall into the trap that it's improper javascripting as that code has been tested successfully outside the project.  I promise it works... just have to figure out how to make it visible.

You may need to research more current versions of full calendar or datetimepicker... I am uncertain.  

Good luck and enjoy.  Make it awesome!!

--Eric Byrne--  7/19/19*@ 

@*I included a unique class name to allow for styling for spacing and visibility... feel free to change or remove if it is needed.*@

<h2 class="employee_timeoffcalendar_h2">Employee Time-Off Calendar</h2>

<div class="row employee_timeoffcalendar_row">
    <div class="col-md-12">
        <div class="employee_timeoffcalendar" id="calendar"></div>
    </div>
</div>

<div id="myModal" class="modal fade" role="dialog">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal">&times;</button>
                <h4 class="modal-title"><span id="eventTitle"></span></h4>
            </div>
            <div class="modal-body">
                <button id="btnDelete" class="btn btn-default btn-sm pull-right">
                    <span class="glyphicon glyphicon-remove"></span> Remove
                </button>
                <button id="btnEdit" class="btn btn-default btn-sm pull-right" style="margin-right:5px;">
                    <span class="glyphicon glyphicon-pencil"></span> Edit
                </button>
                <p id="pDetails"></p>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            </div>
        </div>
    </div>
</div>

<div id="myModalSave" class="modal fade" role="dialog">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal">&times;</button>
                <h4 class="modal-title">Save Event</h4>
            </div>
            <div class="modal-body">
                <form class="col-md-12 formContainer">
                    <input type="hidden" id="hdEventID" value="0" />
                    <div class="form-group">
                        <label>Subject</label>
                        <input type="text" id="txtSubject" class="form-control" />
                    </div>
                    <div class="form-group">
                        <label>Start</label>
						<div class="input-group" id="dtp1">
							@Html.EditorFor(model => model.Start, new { htmlAttributes = new {@id="txtStart", @type = "text", @class = "datepicker" } })
							@Html.ValidationMessageFor(model => model.Start, "", new { @class = "text-danger" })
							<span class="input-group-addon">
								<span class="glypicon glyphicon-calendar"></span>
							</span>
						</div>
                    </div>
                    <div class="form-group">
                        <div class="checkbox">
                            <label><input type="checkbox" id="chkIsFullDay" checked="checked" /> Is Full Day event</label>
                        </div>

                    </div>
                    <div class="form-group" id="divEndDate" style="display:none">
                        <label>End</label>
						<div class="input-group date" id="dtp2">
							@Html.EditorFor(model => model.End, new { htmlAttributes = new { @id = "txtEnd", @type = "text", @class = "datepicker" } })
							@Html.ValidationMessageFor(model => model.End, "", new { @class = "text-danger" })
							<span class="input-group-addon">
								<span class="glypicon glyphicon-calendar"></span>
							</span>
						</div>
                    </div>
                    <div class="form-group">
                        <label>Description</label>
                        <textarea id="txtDescription" rows="3" class="form-control"></textarea>
                    </div>
                    <div class="form-group">
                        <label>Theme Color</label>
                        <select id="ddThemeColor" class="form-control">
                            <option value="">Default</option>
                            <option value="red">Red</option>
                            <option value="blue">Blue</option>
                            <option value="black">Black</option>
                            <option value="green">Green</option>
                        </select>

                    </div>
                    <button type="button" id="btnSave" class="btn btn-success">Save</button>
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                </form>
            </div>
        </div>
    </div>
</div>


<link href="https://cdnjs.cloudflare.com/ajax/libs/fullcalendar/3.4.0/fullcalendar.min.css" rel="stylesheet" />
<link href="https://cdnjs.cloudflare.com/ajax/libs/fullcalendar/3.4.0/fullcalendar.print.css" rel="stylesheet" media="print" />
<link href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-datetimepicker/4.17.47/css/bootstrap-datetimepicker.min.css" rel="stylesheet" />

@section CalendarScripts{
    <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.18.1/moment.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fullcalendar/3.4.0/fullcalendar.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-datetimepicker/4.17.47/js/bootstrap-datetimepicker.min.js"></script>
    <script>

        $(document).ready(function () {
            var events = [];
            var selectedEvent = null;
            FetchEventAndRenderCalendar();
            function FetchEventAndRenderCalendar() {
                events = [];
                $.ajax({
                    type: "GET",
                    url: "/calendar/GetEvents",
                    success: function (data) {
                        $.each(data, function (i, v) {
                            events.push({
                                eventID: v.EventID,
                                title: v.Subject,
                                description: v.Description,
                                start: moment(v.Start),
                                end: v.End != null ? moment(v.End) : null,
                                color: v.ThemeColor,
                                allDay: v.IsFullDay
                            });
                        })

                        GenerateCalendar(events);

                    },
                    error: function (error) {
                        alert('failed');
                    }

                })
            }


            function GenerateCalendar(events) {
                $('#calendar').fullCalendar('destroy');
                $('#calendar').fullCalendar({
                    contentHeight: 400,
                    defaultDate: new Date(),
                    timeFormat: 'h(:mm)a',
                    header: {
                        left: 'prev,next today',
                        center: 'title',
                        right: 'month, basicWeek, basicDay, agenda'
                    },
                    eventLimit: true,
                    eventColor: '#378006',
                    events: events,
                    //Changed name from CalEvent to CalendarEvent
                    eventClick: function (calendarEvent, jsEvent, view) {
                        selectedEvent = calendarEvent;
                        $('#myModal #eventTitle').text(calendarEvent.title);
                        var $description = $('<div/>');
                        $description.append($('<p/>').html('<b>Start:</b>' + calendarEvent.start.format("MM-DD-YYYY hh:mm A")));
                        if (calendarEvent.end != null) {
                            $description.append($('<p/>').html('<b>End:</b>' + calendarEvent.end.format("MM-DD-YYYY hh:mm A")));
                        }
                        $description.append($('<p/>').html('<b>Description:</b>' + calendarEvent.description));
                        $('#myModal #pDetails').empty().html($description);

                        $('#myModal').modal();
                    },
                    selectable: true,
                    select: function (start, end) {
                        selectedEvent = {
                            eventID: 0,
                            title: '',
                            description: '',
                            start: start,
                            end: end,
                            allDay: false,
                            color: ''
                        };
                        openAddEditForm();
                        $('#calendar').fullCalendar('unselect');
                    },
                    editable: true,
                    eventDrop: function (event) {
                        var data = {
                            EventID: event.eventID,
                            Subject: event.title,
                            Start: event.start.format('MM/DD/YYYY hh:mm A'),
                            End: event.end != null ? event.end.format('MM/DD/YYYY hh:mm A') : null,
                            Description: event.description,
                            ThemeColor: event.color,
                            IsFullDay: event.allDay
                        };
                        SaveEvent(data);
                    }

                })



            }

            $('#btnEdit').click(function () {
                //Open modal dialog for edit event
                openAddEditForm();
            })
            $('#btnDelete').click(function () {
                if (selectedEvent != null && confirm('Are you sure you want to remove?')) {
                    $.ajax({
                        type: "POST",
                        url: '/calendar/DeleteEvent',
                        data: { 'eventID': selectedEvent.eventID },
                        success: function (data) {
                            if (data.status) {
                                //Refresh the calendar
                                FetchEventAndRenderCalendar();
                                $('#myModal').modal('hide');
                            }
                        },
                        error: function () {
                            alert('Failed');
                        }
                    })
                }
            })

            $('#dtp1,#dtp2').datetimepicker({
                format: 'MM/DD/YYYY hh:mm A'
            });

            $('#chkIsFullDay').change(function () {
                if ($(this).is(':checked')) {
                    $('#divEndDate').hide();
                }
                else {
                    $('#divEndDate').show();
                }

            });

            function openAddEditForm() {
                if (selectedEvent != null) {
                    $('#hdEventID').val(selectedEvent.eventID);
                    $('#txtSubject').val(selectedEvent.title);
                    $('#txtStart').val(selectedEvent.start.format('MM/DD/YYYY hh:mm A'));
                    $('#chkIsFullDay').prop("checked", selectedEvent.allDay || false);
                    $('#chkIsFullDay').change();
                    $('#txtEnd').val(selectedEvent.end != null ? selectedEvent.end.format('MM/DD/YYYY hh:mm A') : '');
                    $('#txtDescription').val(selectedEvent.description);
                    $('#ddThemeColor').val(selectedEvent.color);
                }
                $('#myModal').modal('hide');
                $('#myModalSave').modal();
            }

            $('#btnSave').click(function () {
                //Validation/
                if ($('#txtSubject').val().trim() == "") {
                    alert('Subject required');
                    return;
                }
                if ($('#txtStart').val().trim() == "") {
                    alert('Start date required');
                    return;
                }
                if ($('#chkIsFullDay').is(':checked') == false && $('#txtEnd').val().trim() == "") {
                    alert('End date required');
                    return;
                }
                else {
                    var startDate = moment($('#txtStart').val(), "MM/DD/YYYY hh:mm A").toDate();
                    var endDate = moment($('#txtEnd').val(), "MM/DD/YYYY hh:mm A").toDate();
                    if (startDate > endDate) {
                        alert('Invalid end date');
                        return;
                    }

                }

                var data = {
                    EventId: $('#hdEventID').val(),
                    Subject: $('#txtSubject').val().trim(),
                    Start: $('#txtStart').val().trim(),
                    End: $('#chkIsFullDay').is(':checked') ? null : $('#txtEnd').val().trim(),
                    Description: $('#txtDescription').val(),
                    ThemeColor: $('#ddThemeColor').val(),
                    IsFullDay: $('chkIsFullDay').is(':checked')
                }
                SaveEvent(data);
                //..........................................
                // call function for submit data to the server

            })

            function SaveEvent(data) {
                $.ajax({
                    type: "POST",
                    url: '/calendar/SaveEvent',
                    data: data,
                    success: function (data) {
                        if (data.status) {
                            //Refresh the calendar
                            FetchEventAndRenderCalendar();
                            $('#myModalSave').modal('hide');
                        }
                    },
                    error: function () {
                        alert('Failed');
                    }
                })

            }

        })
    </script>
}



