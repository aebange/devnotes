### How to display information from a back-end database on the frontend in Angular
In this example, we pull image URLs from the database.
It is assumed that a database already exists and is connected to angular

_____

### Set up the back-end to read information from the database
1. Create a DTO to hold the information from the database sql
    1. ``cd backend``
    2. ``ng g class models/UrlDTO``
    3. Edit the contents of the DTO, in this example ours looks like this...
    ```java
    package com.lessons.models;

    import com.fasterxml.jackson.annotation.JsonProperty;

    import java.text.Format;
    import java.text.SimpleDateFormat;
    import java.util.Date;

    public class UrlDTO {

       @JsonProperty("id")
       private Integer id;

       @JsonProperty("url")
       private String url;

       @JsonProperty("timestamp")
       private String timestamp;

       public Integer getId() {
           return id;
       }

       public void setId(Integer id) {
           this.id = id;
       }

       public String getUrl() {
           return url;
       }

       public void setUrl(String url) {
           this.url = url;
       }

        public String getTimestamp() {
            return timestamp;
        }

        public void setTimestamp(String timestamp) {
            this.timestamp = timestamp;
        }
   }
   ```
2. Create a service to fetch our data from the database
    1. ``cd backend``
    2. ``ng g class services/UrlService``
    3. When editing this, it's best to return fake data first - worry about the sql later
    3. Edit the contents of the service, in this example ours looks like this...
    ```Java
    package com.lessons.services;

    import com.lessons.models.UrlDTO;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.stereotype.Service;

    import javax.annotation.Resource;
    import javax.sql.DataSource;
    import java.util.ArrayList;
    import java.util.List;

    @Service("com.lessons.services.UrlService")
    public class UrlService {
        private static final Logger logger = LoggerFactory.getLogger(UrlService.class);

        @Resource
        private DataSource dataSource;

        // A fake return, this will be proper sql later
        public List<UrlDTO> getAllUrls() {
        logger.debug("getAllUrls started in the back-end");
            UrlDTO dto1 = new UrlDTO();
            dto1.setId(1);
            dto1.setUrl("https://i.imgur.com/Ik0KvPo.png");
            dto1.setTimestamp("5 o clock");

            UrlDTO dto2 = new UrlDTO();
            dto2.setId(2);
            dto2.setUrl("https://i.imgur.com/FvmRwBF.png");
            dto2.setTimestamp("5 o clock");

            UrlDTO dto3 = new UrlDTO();
            dto3.setId(3);
            dto3.setUrl("https://i.imgur.com/2om7PNe.png");
            dto3.setTimestamp("5 o clock");

            List<UrlDTO> dtoList = new ArrayList<UrlDTO>();
            dtoList.add(dto1);
            dtoList.add(dto2);
            dtoList.add(dto3);

            return dtoList;
        }
    }
    ```
3. Create a controller to receive the REST call from the front-end
    1. ``cd backend``
    2. ``ng g class controllers/UrlController``
    3. Edit the contents of the controller, in this example ours looks like this...
    ```java
    package com.lessons.controllers;

    import com.lessons.models.UrlDTO;
    import com.lessons.services.UrlService;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.http.HttpStatus;
    import org.springframework.http.ResponseEntity;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;

    import javax.annotation.Resource;
    import java.util.List;

    @Controller
    public class UrlController {
        private static final Logger logger = LoggerFactory.getLogger(UrlController.class);

        // MAKE SURE TO REFERENCE THE SERVICE HERE OR YOU WILL HAVE ISSUES WITH STATIC METHODS
        @Resource
        private UrlService urlService;

        @RequestMapping(value="/api/url", method = RequestMethod.GET, produces = "application/json")
        public ResponseEntity<?> getUrlList() {
            logger.debug("getUrlList started in the back-end following a REST call from the front-end");

            // Get the List of URLs
            List<UrlDTO> dtoList = urlService.getAllUrls();

            return ResponseEntity
                    .status(HttpStatus.OK)
                    .body(dtoList);
        }

    }
    ```
4. Test the changes we've made to the back-end
    1. Debug the back-end only, drop break-points if you must
    2. Hit the REST endpoint with POSTMAN to simulate the front-end making the call
    3. The outcome should look like this
    [![Demo CountPages alpha](https://i.imgur.com/3p5fmLN.gif)](https://i.imgur.com/3p5fmLN.gif)
    4. Remember what your api path is and what your fake data looks like

5. In a database console, write a working SQL query to get the data you want to display
    1. In this case, we are using ``SELECT * FROM urls``

6. Modify the UrlService to actually query the database and not use fake data
    1. Modify the public method to do something like this...
    ```java
    // Returns a list of all the entries in the 'urls' table in the database
    public List<UrlDTO> getAllUrls() {
        logger.debug("getAllUrls started in the back-end");

        String sql = "SELECT * FROM urls";

        BeanPropertyRowMapper<UrlDTO> rowMapper = new BeanPropertyRowMapper<>(UrlDTO.class);

        JdbcTemplate jt = new JdbcTemplate(this.dataSource);

        List<UrlDTO> listOfUrls = jt.query(sql, rowMapper);

        return listOfUrls;
    }
    ```
7. Test the changes we've made to the back-end (again)
    1. Debug the back-end only, drop break-points if you must
    2. Hit the REST endpoint with POSTMAN to simulate the front-end making the call
    3. Your outcome should contain actual data from the database this time
        1. If your database doesn't have actual data, add some temp data with some sql in the database console

8. At this point the back-end should be done, now we need to make changes to the front-end

### Set up the front-end to read information from the database
1. In this guide, we're assuming you've already got a page - but in case you don't, review [this](https://github.com/aebange/devnotes/blob/master/Learning%20Angular/Creating%20pages%20in%20Angular%20with%20security.md)
2. Create a front-end DTO to store the DTO from the back-end
    1. ``cd frontend``
    2. ``ng g model models/url-dto --skipTests``
    3. Edit the url-dto.ts, the contents will look like this...
    ```typescript
    export class UrlsDto {

      public id: number;

      public url: string;

      public timestamp: string;

    }
    ```
3. Create a front-end service to invoke the REST call to get the DTO from the back-end and store it in our front-end DTO
    1. ``cd frontend``
    2. ``ng g service services/urls --skipTests`` (Remember, angular adds '.service.ts' to the end, so keep 'service' out of the name)
    3. Edit the file, the contents will look like this...
    ```typescript
   import { Injectable } from '@angular/core';
   import {HttpClient} from "@angular/common/http";
   import {UrlDTO} from "../models/url-dto";
   import {Observable} from "rxjs";
   import {environment} from "../../environments/environment";

   @Injectable({
     providedIn: 'root'
   })
   export class UrlService {

     constructor(private httpClient: HttpClient) { }

     // Make a REST call to the back-end to get a list of UrlsDTO
     public getAllusers(): Observable<UrlDTO[]> {

       const restUrl = environment.baseUrl + '/api/geturls'

       return this.httpClient.get <UrlDTO[]> (restUrl)
     }
   }
   ```
### Connect your service to the html of your page
1. Inject the service into the typescript of the page
    1. Create a variable to store your observable
    2. Inject the service into the constructor
    3. The outcome should be something along the lines of...
    ```typescript
   public obsUrlList: Observable<UrlDTO[]>

   constructor(public urlService: UrlService)
   ```

2. Edit the HTML to load the output
    1. You SHOULD store the rendered page elements inside of something with an async pipe to prevent rendering them until the information is ready
    ```html
    <!--The async pipe will delay rendering till we get a response-->
    <ng-container *ngIf="(this.obsUrlList | async) as urls">
        <div fxLayout="column" fxLayoutGap="20px">
            <!--The header row-->
            <div class="header" fxLayout="row" fxLayoutGap="100px">
                <div fxFlex="20%">
                    id
                </div>
                <div fxFlex="20%">
                    url
                </div>
                <div fxFlex="20%">
                    timestamp
                </div>
            </div>

            <!--The return from the database, iterate through with a for loop row-->
            <div class="contents" fxLayout="row" fxlayoutGap="100px" *ngFor="let url of urls">
                <div fxFlex="20%">
                    {{ url.id }}
                </div>
                <div fxFlex="20%">
                    {{ url.url }}
                </div>
                <div fxFlex="20%">
                    {{ url.timestamp }}
                </div>
            </div>
        </div>
    </ng-container>
    ```

3. At this point you should have a working page, good luck