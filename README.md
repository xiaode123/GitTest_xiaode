# GitTest_xiaode
package com.mini.web.controller.appinterface.v7;

import com.mini.biz.auth.dto.StatefulForm;
import com.mini.biz.homepage.dto.SelectComFunForm;
import com.mini.biz.homepage.service.HomeFuncService;
import com.mini.biz.homepage.service.HomeShowService;
import com.mini.biz.loan.service.LoanService;
import com.mini.common.config.InterfaceLog;
import com.mini.common.config.RequireLogin;
import com.mini.common.utils.JsonFormValidator;
import com.mini.common.utils.JsonResult;
import com.mini.dal.mapper.ext.HomeFuncExtMapper;
import com.mini.dal.mapper.ext.UserFuncExtMapper;
import com.mini.dal.model.HomeFuncDO;
import com.mini.dal.model.UserDO;
import com.mini.dal.model.UserFuncDO;
import com.mini.dal.query.HomeFuncQuery;
import com.mini.dal.query.UserFuncQuery;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.util.CollectionUtils;
import org.springframework.validation.Errors;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.validation.Valid;
import java.util.Date;
import java.util.List;

/**
 * @author hukaisheng
 */
@Controller("homePageShowV7")
@RequestMapping("/v7/homePage")
public class HomePageController {

    @Autowired
    private LoanService loanService;

    @Autowired
    private HomeShowService homeShowService;

    @Autowired
    private HomeFuncService homeFuncService;

    @Autowired
    private HomeFuncExtMapper homeFuncExtMapper;

    @Autowired
    private UserFuncExtMapper userFuncExtMapper;

    /**
     * 首页信息展示
     *
     * @return
     */
    @InterfaceLog(interName = "v7/homePage/show.json")
    @ResponseBody
    @RequireLogin
    @RequestMapping(value = "/show.json", method = RequestMethod.POST)
    public Object show(@Valid @RequestBody StatefulForm form, Errors errors) {
        JsonResult jsonResult = JsonFormValidator.validate(errors);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }
        //通过sessionId校验用户是否存在，如果存在返回用户信息
        UserDO userDO = loanService.validateUserBySession(form.getSessionId(), jsonResult);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }

        jsonResult.setResult(homeFuncService.showHome(userDO, form.getTTID()));

        return jsonResult.getMap();
    }

    /**
     * 首页信息展示
     *
     * @return
     */
    @InterfaceLog(interName = "v7/homePage/allFuns.json")
    @ResponseBody
    @RequireLogin
    @RequestMapping(value = "/allFuns.json", method = RequestMethod.POST)
    public Object allFuns(@Valid @RequestBody StatefulForm form, Errors errors) {
        JsonResult jsonResult = JsonFormValidator.validate(errors);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }
        //通过sessionId校验用户是否存在，如果存在返回用户信息
        UserDO userDO = loanService.validateUserBySession(form.getSessionId(), jsonResult);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }

        jsonResult.setResult(homeFuncService.allFuncs(userDO, form.getTTID()));

        return jsonResult.getMap();
    }

    /**
     * 首页信息展示
     *
     * @return
     */
    @InterfaceLog(interName = "v7/homePage/selectComFun.json")
    @ResponseBody
    @RequireLogin
    @RequestMapping(value = "/selectComFun.json", method = RequestMethod.POST)
    public Object selectComFun(@Valid @RequestBody SelectComFunForm form, Errors errors) {
        JsonResult jsonResult = JsonFormValidator.validate(errors);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }
        if (CollectionUtils.isEmpty(form.getComFuns())) {
            jsonResult.setErrMsg("请选择您需要的常用服务");
            return jsonResult.getMap();
        }
        if (form.getComFuns().size() > 5) {
            jsonResult.setErrMsg("常用服务最多不超过5个");
            return jsonResult.getMap();
        }
        //通过sessionId校验用户是否存在，如果存在返回用户信息
        UserDO userDO = loanService.validateUserBySession(form.getSessionId(), jsonResult);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }
        homeFuncService.comFun(userDO,form.getComFuns());

        return jsonResult.getMap();
    }

    /**
     * 首页信息展示
     *
     * @return
     */
    @InterfaceLog(interName = "v7/homePage/dms.json")
    @ResponseBody
    @RequireLogin
    @RequestMapping(value = "/dms.json", method = RequestMethod.POST)
    public Object dms(@Valid @RequestBody StatefulForm form, Errors errors) {
        JsonResult jsonResult = JsonFormValidator.validate(errors);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }
        //通过sessionId校验用户是否存在，如果存在返回用户信息
        UserDO userDO = loanService.validateUserBySession(form.getSessionId(), jsonResult);
        if (!jsonResult.isSuccess()) {
            return jsonResult.getMap();
        }

        jsonResult.setResult(homeFuncService.dmsFuncs(userDO, form.getTTID()));

        return jsonResult.getMap();
    }
}
