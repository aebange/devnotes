###Subjects:
- How to generate components with Angular (ng)
- How to connect newly generated component paths/routes to a constant
- How to connect components and their routes to security methods (PageGuard)
- How to grant specific roles permission to access components through the PageGuard
- How to add the component to the navbar
____________________________

1. Generate the component with ng
    ```shell
    cd ~/IntellijProjects/angularApp1/frontend
   ```
    ```shell
    ng generate component OBJECTNAME --skipTests
   ```

2. Create a constant for the path to the page
    Open constants.ts
    Add a line:
        ```ts
        OBJECTNAME_ROUTE = "page/OBJECTNAME"
        ```

3. Add the page into security
    Navigate to app.module.ts
    Add the following line to the imports at the top of the document
    ```ts
    import { OBJECTNAMEComponent } from '/OBJECTNAME.component;'
    ```   
    *Note: The OBJECTNAMEComponent is automatically generated in OBJECTNAME.component.ts by ng create*
    Add the following line under const appRoutes: Routes = [...
    ```ts
    { path: Constants.OBJECTNAME_ROUTE,                        component: OBJECTNAMEComponent, canActivate: [PageGuard] },
    ```
    What are we doing here? We're importing the automatically generated OBJECTNAMEComponent and our route from the constant and we're connecting it to the PageGuard which determines if the user can PROCEED to the to the route - this handles role permissions, 

    Now that we've got our OBJECTNAMEComponent and path connected to the PageGuard (which we need to do for the page to be accessible), we need to tell PageGuard which roles can access the page
   
    In order to do this, we need to assign an ID to the page and then feed the roles_uicontrols table a role_id along with our object's custom ID (in this case 14000)

    In this case, we will be assigning access permission to role '4' which can be found under the roles table in the db. Role '4' refers to row #4 - the "NCCS_SUPERUSER"

    Navigate to db-migrations/src/main/resources/db_migration
    Create a new sql file to insert the page into the prospective databases
        **sql files must be named in the format "V#.#__something.sql"**
        **DO NOT edit currently existing sql files in this directory, new ones must be created**
    In the new sql file add the lines
        *(In this case )
    ```sql
    insert into uicontrols(id, name) values(14000, 'page/OBJECTNAME';
    ```
   *Note: The '14000' represents the desired ID for this (it doesn't exist before this is run)*
   ```sql
    insert into roles_uicontrols(role_id, uicontrols_id) values (4, 14000);``
    ```
    *Note: The '4' represents the row that this role rests in in our role database table, and the '14000' comes from the line above*

4. Add the component to the navbar
   Add the following lines to frontend>src>layout>navbar>navbar.component.html
   ```html
   <mat-list-item class="navItem" [routerLink]="constants.OBJECTNAME_ROUTE" routerLinkActive="active"
                 *ngIf="userInfoDTO.pageRoutes.get(constants.OBJECTNAME_ROUTE)">
     <a title="OBJECTNAME">OBJECTNAME</a>
     <div fxFlex fxLayoutAlign="end end">
       <a [routerLink]="constants.OBJECTNAME_ROUTE" target="_blank">
         <i class="fas fa-external-link-alt navItemIcon" title="Open OBJECTNAME in a new window"></i>
       </a>
     </div>
   </mat-list-item>
   ``` 