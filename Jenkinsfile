
@Library('roboshop-shared-lib') _

dev catalogueMap = [
    component: "catalogue",
    application: "nodejsVM"
];
// println("env" + env); 
if(!env.BRANCH_NAME.equalsIgnoreCase('main')){
    pipelineDecision.decidePipeline(catalogueMap);
}else{

}
