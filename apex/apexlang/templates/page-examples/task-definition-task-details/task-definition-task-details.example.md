---
templateId: page-examples.task-definition-task-details.page.example
componentType: markdown-apexlang-example
version: 1.0
migrationNote: preserved from previous standalone template example
---

# Task Definition Task Details Example

## Purpose

Markdown-preserved APEXlang example. Use for syntax/structure only after loading family `_index.md` and `_common.md`.

## Example

```apexlang
page 10 (
    name: Task Details
    title: Task Details
    alias:TASK-DETAILS
    appearance {
        pageMode: modalDialog
        dialogTemplate: @/drawer
        templateOptions: [
            #DEFAULT#
            js-dialog-class-t-Drawer--pullOutEnd
        ]
    }
    dialog {
        resizable: false
    }
    javaScript {
        functionAndGlobalVarDeclaration:
            ```javascript-browser
            apex.jQuery(() => {
                apex.jQuery("a.taskActionMenu").each((index, item) => {
                    const element = apex.jQuery(item);
                    const actionName = decodeURI(element.attr("href")).match(/\\$([^?]+)/)[1];
                    const actionLabel = element.text();
                    apex.actions.add({
                        name: actionName,
                        label: actionLabel,
                        action: (event, element, args) => {
                            if (args.do === "submit") apex.page.submit(actionName.toUpperCase());
                            else if (args.do === "openRegion") apex.theme.openRegion(actionName.toUpperCase());
                        }
                    });
                });
            });
            ```
    }
    security {
        pageAccessProtection: argumentsMustHaveChecksum
        formAutoComplete: false
    }
    advanced {
        enableDuplicatePageSubmissions: false
    }

    region add-comment (
        name: Add Comment
        type: staticContent
        layout {
            sequence: 100
            parentRegion: @comments
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_add_comment )
                ```
        }
        settings {
            outputAs: text
        }
    )

    region buttons (
        name: Buttons
        type: staticContent
        layout {
            sequence: 80
            slot: REGION_POSITION_03
        }
        appearance {
            template: @/buttons-container
            templateOptions: [
                #DEFAULT#
                t-ButtonRegion--stickToBottom
                t-ButtonRegion--slimPadding
                margin-bottom-none
            ]
        }
        advanced {
            regionDisplaySelector: true
        }
        settings {
            outputAs: text
        }
    )

    region cancel-task (
        name: Cancel Task
        type: staticContent
        source {
            htmlCode:
                ```html
                <p>Do you really want to cancel this task?</p>
                <p>This will mark the task as no longer needed.</p>
                ```
        }
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: CANCEL
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_cancel )
                ```
        }
    )

    region comments (
        name: Comments
        type: themeTemplateComponent/comments
        source {
            location: localDatabase
            type: sqlQuery
            sqlQuery:
                ```sql
                select apex_string.get_initials(created_by) as user_initials,
                       'u-color-'||ora_hash(created_by,45)  as user_css_class,
                       created_by                           as user_name,
                       text                                 as comment_text,
                       created_on                           as comment_date
                  from apex_task_comments
                 where nvl(:P10_ALL_COMMENTS, 'N') = 'N'
                       and task_id = :P10_TASK_ID
                    or nvl(:P10_ALL_COMMENTS, 'N') = 'Y'
                       and task_id in (
                                select task_id
                                  from apex_tasks
                               connect by prior previous_task_id = task_id
                                 start with task_id = :P10_TASK_ID )
                 order by created_on desc
                ```
            pageItemsToSubmit: [
                P10_TASK_ID
                P10_ALL_COMMENTS
            ]
        }
        layout {
            sequence: 60
            slot: BODY
        }
        appearance {
            template: @/collapsible
            templateOptions: [
                #DEFAULT#
                js-useLocalStorage
                t-Region--scrollBody
            ]
            renderComponents: belowContent
        }
        accessibility {
            landmarkType: region
        }
        componentAppearance {
            display: report
        }
        settings {
            commentText: COMMENT_TEXT
            userName: USER_NAME
            date: COMMENT_DATE
            style: chatSpeechBubbles
            applyThemeColors: false
        }
        plugin-avatar {
            avatarType: initials
            avatarInitials: USER_INITIALS
            avatarDescription: &USER_NAME.
            avatarShape: circular
            avatarSize: medium
            avatarCssClasses: &USER_CSS_CLASS.
        }
        messages {
            whenNoDataFound: No Comments
            noDataFoundIcon: fa-comments-o fa-lg
        }

        column COMMENT_DATE (
            layout {
                sequence: 50
            }
            source {
                databaseColumn: COMMENT_DATE
                dataType: date
            }
            accessibility {
                valueIdentifiesRow: true
            }
            appearance {
                formatMask: SINCE
            }
        )

        column COMMENT_TEXT (
            layout {
                sequence: 40
            }
            source {
                databaseColumn: COMMENT_TEXT
                dataType: varchar2
            }
        )

        column USER_CSS_CLASS (
            layout {
                sequence: 20
            }
            source {
                databaseColumn: USER_CSS_CLASS
                dataType: varchar2
            }
        )

        column USER_INITIALS (
            layout {
                sequence: 10
            }
            source {
                databaseColumn: USER_INITIALS
                dataType: varchar2
            }
        )

        column USER_NAME (
            layout {
                sequence: 30
            }
            source {
                databaseColumn: USER_NAME
                dataType: varchar2
            }
            accessibility {
                valueIdentifiesRow: true
            }
        )
    )

    region delegate (
        name: Delegate
        type: staticContent
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: DELEGATE
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_delegate )
                ```
        }
    )

    region details (
        name: Details
        type: themeTemplateComponent/details
        source {
            location: localDatabase
            type: sqlQuery
            sqlQuery:
                ```sql
                select task_id,
                       title,
                       description,
                       status,
                       priority,
                       due_date,
                       created_by,
                       created_on
                  from apex_tasks
                 where task_id = :P10_TASK_ID
                ```
            pageItemsToSubmit: [
                P10_TASK_ID
            ]
        }
        layout {
            sequence: 20
            slot: BODY
        }
        appearance {
            template: @/collapsible
            templateOptions: [
                #DEFAULT#
                js-useLocalStorage
                t-Region--scrollBody
            ]
            renderComponents: belowContent
        }
        accessibility {
            landmarkType: region
        }
        componentAppearance {
            display: report
        }
        settings {
            label1: TITLE
            value1: TITLE
            label2: DESCRIPTION
            value2: DESCRIPTION
            label3: STATUS
            value3: STATUS
            label4: PRIORITY
            value4: PRIORITY
            label5: DUE_DATE
            value5: DUE_DATE
        }
        messages {
            whenNoDataFound: No Details
        }
    )

    region developer-information (
        name: Developer Information
        type: staticContent
        source {
            htmlCode:
                ```html
                <div class="apex-gridspace">
                    <div class="apex_cells apex_cell apex_colspan-12 apex_alignment-center">
                        <span class="apex_labels">
                            <span class="apex_label">Task ID</span>
                            <span class="apex_value">&P10_TASK_ID.</span>
                        </span>
                    </div>
                </div>
                ```
        }
        layout {
            sequence: 100
            parentRegion: @details
            slot: SUB_REGIONS
        }
        appearance {
            template: @/interactive-report
            templateOptions: [
                #DEFAULT#
                t-Region--scrollBody
            ]
        }
        settings {
            outputAs: text
        }
    )

    region due (
        name: Due
        type: staticContent
        source {
            htmlCode:
                ```html
                <span class="apex_labels">
                    <span class="apex_label">Due Date</span>
                    <span class="apex_value">&P10_DUE_DATE.</span>
                </span>
                ```
        }
        layout {
            sequence: 40
            slot: REGION_POSITION_03
        }
        appearance {
            template: @/blank-with-attributes
            templateOptions: #DEFAULT#
        }
        settings {
            outputAs: text
        }
    )

    region edit-parameter (
        name: Edit Parameter
        type: staticContent
        source {
            htmlCode:
                ```html
                <div class="apex_gridspace">
                    <div class="apex_cells apex_cell apex_colspan-12">
                        <span class="apex_labels">
                            <span class="apex_label">Parameter</span>
                            <span class="apex_value">&P10_PARAMETER.</span>
                        </span>
                    </div>
                </div>
                ```
        }
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: EDIT_PARAMETER
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_edit_parameter )
                ```
        }
    )

    region history (
        name: History
        type: themeTemplateComponent/history
        source {
            location: localDatabase
            type: sqlQuery
            sqlQuery:
                ```sql
                select created_by as user_name,
                       created_on as action_date,
                       text as action_description
                  from apex_task_history
                 where task_id = :P10_TASK_ID
                 order by created_on desc
                ```
            pageItemsToSubmit: [
                P10_TASK_ID
            ]
        }
        layout {
            sequence: 90
            slot: REGION_POSITION_03
        }
        appearance {
            template: @/collapsible
            templateOptions: [
                #DEFAULT#
                js-useLocalStorage
                t-Region--scrollBody
            ]
            renderComponents: belowContent
        }
        accessibility {
            landmarkType: region
        }
        componentAppearance {
            display: report
        }
        settings {
            userName: USER_NAME
            date: ACTION_DATE
            description: ACTION_DESCRIPTION
        }
        messages {
            whenNoDataFound: No History
        }
    )

    region invite-participant (
        name: Invite Participant
        type: staticContent
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: INVITE
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_invite_participant )
                ```
        }
    )

    region priority (
        name: Priority
        type: staticContent
        source {
            htmlCode:
                ```html
                <span class="apex_labels">
                    <span class="apex_label">Priority</span>
                    <span class="apex_value">&P10_PRIORITY.</span>
                </span>
                ```
        }
        layout {
            sequence: 30
            slot: REGION_POSITION_03
        }
        appearance {
            template: @/blank-with-attributes
            templateOptions: #DEFAULT#
        }
        settings {
            outputAs: text
        }
    )

    region remove-participant (
        name: Remove Participant
        type: staticContent
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: REMOVE
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_remove_participant )
                ```
        }
    )

    region request-information (
        name: Request Information
        type: staticContent
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: REQUEST_INFO
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_request_information )
                ```
        }
    )

    region subject (
        name: Subject
        type: themeTemplateComponent/subject
        source {
            location: localDatabase
            type: sqlQuery
            sqlQuery:
                ```sql
                select task_id,
                       title,
                       description
                  from apex_tasks
                 where task_id = :P10_TASK_ID
                ```
            pageItemsToSubmit: [
                P10_TASK_ID
            ]
        }
        layout {
            sequence: 10
            slot: BODY
        }
        appearance {
            template: @/collapsible
            templateOptions: [
                #DEFAULT#
                js-useLocalStorage
                t-Region--scrollBody
            ]
            renderComponents: belowContent
        }
        accessibility {
            landmarkType: region
        }
        componentAppearance {
            display: report
        }
        settings {
            label1: TITLE
            value1: TITLE
            label2: DESCRIPTION
            value2: DESCRIPTION
        }
        messages {
            whenNoDataFound: No Subject
        }
    )

    region submit-information (
        name: Submit Information
        type: staticContent
        layout {
            sequence: 80
            parentRegion: @subject
            slot: SUB_REGIONS
        }
        appearance {
            template: @/inline-popup
            templateOptions: [
                #DEFAULT#
                js-dialog-autoheight
                js-dialog-nosize
            ]
        }
        accessibility {
            landmarkType: form
        }
        advanced {
            htmlDomId: SUBMIT_INFO
        }
        serverSideCondition {
            type: expression
            plsqlExpression:
                ```plsql
                apex_human_task.is_allowed (
                    p_task_id   => :P10_TASK_ID,
                    p_operation => apex_human_task.c_task_op_submit_information )
                ```
        }
    )

    process close-dialog (
        name: Close Dialog
        type: closeDialog
        execution {
            sequence: 999
            point: afterSubmit
        }
    )
)

```
